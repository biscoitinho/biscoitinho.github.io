---
layout: post
title: "Cpu monitor with Python"
date: 2020-01-05 19:35:00
categories: python
---

Welcome in 2020!


Recently I have a problem with my laptop, Cpu is overheating. Well, nothing major that
ol' cleaning the internals and a new thermo paste dose won't do, but till then
I've wrote a little script to monitor the situation in my cores.

```
#!/usr/bin/python

'''Automation script for CPU warning level temperatures and logging'''

import os
import datetime
import time

class CpuTemp:
    '''Cpu Temperature monitoring utility'''
    SYS_TEMP = os.popen("cat /sys/class/thermal/thermal_zone*/temp").read()

    @classmethod
    def temp_list(cls, systemtemp):
        '''Convert system temp to a list of ints'''
        dump_list = []
        dump_list.append(systemtemp)
        t_list = dump_list[0].split("\n")
        del t_list[-1]
        numbers = map(int, t_list)

        return list(numbers)

    @classmethod
    def log_temp(cls, temp):
        '''Log the warning levels to a file'''
        now = datetime.datetime.now().strftime("%c")
        log_file = open("temp_log.txt", "a")
        log_file.write(str(now) + " " + "CPU temp at " + str(temp / 1000) + " degrees." + "\n")
        log_file.close()


if __name__ == "__main__":
    starttime=time.time()
    while True:
        CT = CpuTemp()
        for i in CT.temp_list(CpuTemp.SYS_TEMP):
            if i > 85000:
                os.system("notify-send CPU_temp_at_warning_level")
                CT.log_temp(i)
        time.sleep(60.0 - ((time.time() - starttime) % 60.0))
```

Whole thing is actually based on two principles:


One: It must be a linux operating system on which this command works:
`cat /sys/class/thermal/thermal_zone*/temp`
It displays your current cpu temperature in mC (miliCelsius).
Values will differ depending on hardware but mine for instance were something like this:

```
76000
29000
79000
```

Quoting comarade Diatlov - Not great but not terrible.
Also as for Diatlov situation here tends to get more dramatic because those above readings are
when only the terminal is working and 4 bookmarks (static pages opend) in Firefox, it gets messy
really fast and really bad when going for instance to facebook. Clearly something has to be done here.

Two: There is a `notify-send` utility available in your distro. That's for notifications, but if there
is no possibility of it I will also point out later on an alternative solution.


Let's disect the script:


Three imports for starters, plain common python ones, no need for additional fancy libs here.
Then the main script class and a `SYS_TEMP` constant which takes the data from the mentioned linux command.
Then the `temp_list` method. Basically what it does it converts the integers provided from the command (example above)
and places them in a list (of integers). Probably this can be done simplier, clearier and fancier, but this also works.


Next the `log_temp` method. It takes a `temp` argument and opens a file and writes the time and the temperature there.
Note the conversion in the print from miliCelsius to Celsius - I don't need this amout of precision here.


Now the call. Here it's a little tricky. We are locking a time loop to a system time, therfore making a little 'cron' thing
to tell the script when the call should occur. In our case every minute (see last line). Next inside the while loop is 'for' loop
and an if statement which checks if the temperature is above 85 celsius and then if it is the notification pops up and the
data is logged in the file.


Now if you don't have or don't want a `notify-send` function you can insted make a system call `tail temp_log.txt` and you'll
get a print out of the log file in the teminal.


Next step maybe a logic to control a computers fan when the temperature exceeds the warning levels?
And yes, I know there are utilities that do this for me and even more and it can be also done in bash with a normal cron
job, but I did it like Sinatra - my way.


Cheers
