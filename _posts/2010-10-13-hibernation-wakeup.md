---
layout: post
title: Scheduled hibernation and wakeup
tags: [windows,batch]
---

Go to Power Options in Control Panel and create a power profile called "day_power" that is set to never standby or hibernate, and a power profile called "night_power" that is set to hibernate after 1 minute.

Create a batch file called day_power.bat containing:

```
%windir%\system32\powercfg.exe /S day_power
```

Create a batch file called night_power.bat containing:

```
%windir%\system32\powercfg.exe /S night_power
```

Create a Scheduled Task called "day_power" with the following options:
Program: day_power.bat
Schedule: Daily at 8:00AM
Other settings: Wake up the computer to run this task

Create a Scheduled Task called "night_power" with the following options:
Program: night_power.bat
Schedule: Daily at 10:00PM
Other settings: Wake up the computer to run this task 