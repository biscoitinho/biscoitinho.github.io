---
layout: post
title: "Notes TUI for Omarchy"
date: 2026-01-20 09:00:00
categories: ruby
---

Hi,

Recently I'm in love with Omarchy linux, I wanted naturally to improve my workflow in the system.
Also I have discovered an awesome youtube channel [BreadAndPenguins](https://www.youtube.com/@BreadOnPenguins)
Combining this two together and watching this video: [Dmenu Magic](https://www.youtube.com/watch?v=4JWeU78A95c)
I was amazed by the functionality and simplicity of notes taking using dmenu.
Trying to implement it in Omarchy gave me a couple of problems plus it was visually a lot different than what is a default
esthetic of the system, so I decided I'll rewrite it in Ruby plus use the utilities already in Omarchy which will
give it the same looks and it will feel seamless and natural to use it.

Here is the code:

```
#!/usr/bin/env ruby
require "fileutils"

FOLDER = File.expand_path("~/notes/")
EDITOR = "nvim"
TERM   = "alacritty"

def walker_dmenu(items, prompt)
  IO.popen(["walker", "--dmenu", "-p", prompt], "r+") do |io|
    io.puts items
    io.close_write
    io.read.chomp
  end
end

def walker_input(prompt)
  IO.popen(["walker", "--dmenu", "--placeholder", prompt, "-p", prompt], "r+") do |io|
    io.close_write
    io.read.chomp
  end
end

def open_editor(path)
  FileUtils.touch(path)
  Process.detach(Process.spawn(TERM, "msg", "create-window", "-e", EDITOR, path))
end

def newnote
  dirs = Dir.glob("#{FOLDER}/*/").prepend(FOLDER + "/")
  dir  = walker_dmenu(dirs, "Choose directory: ")
  exit if dir.empty?

  name = walker_input("Enter a name (leave empty for timestamp): ")
  name = Time.now.strftime("%F_%H-%M-%S") if name.empty?
  name += ".md" unless name.end_with?(".md")

  open_editor(File.join(dir, name))
end

def selected
  notes = Dir.glob("#{FOLDER}/**/*.md")
              .map    { |f| [File.mtime(f), f] }
              .sort   { |a, b| b[0] <=> a[0] }
              .map    { |_, f| f.sub("#{FOLDER}/", "") }

  choice = walker_dmenu(["New"] + notes, "Choose note or create new: ")

  case choice
  when "New"   then newnote
  when /\.md$/ then open_editor(File.join(FOLDER, choice))
  end
end

FileUtils.mkdir_p(FOLDER)
selected
```

I've used walker to be visually the same as all other TUI stuff in Omarchy, plus you can choose which terminal or editor you ar running.
What it does? Well Bread explained it better, but basically you hit a keybinding of your choosing (in my case SUPER + N) then you are
promted to choose a name of the note, if none will be given then it will be created with current date. Next you'll need to choose the
folder where it should be placed. Afterwards editor of your choosing will be opened and you will be ready to take notes.
Super simple, super convinient. I use it on daily basis to track my progress or to just write somhere my thoughts and ideas.


Cheers
