---
layout: post
title: Drive letter
tags: [windows,batch]
---

To get the current drive letter:

```bat
REM The following lines determine the current drive letter
echo @echo off> volume.bat
echo set getdrv_=%%3>> volume.bat
dir | find "Volume"> go.bat
call go
if exist volume.bat del volume.bat
if exist go.bat del go.bat
::
rem show that we got it
echo Determined drive letter: %getdrv_%
pause
echo Another way: %~d0
pause
```

To assign the same drive letter to access files in a usb drive, create a batch file containing:

```bat
@echo off
subst %~d0 V:\
```

and save it to your Portable. The SUBST command is designed to substitute something like "C:\MyLotsOfFolders\OneOfThem\ThisOne\IMeanThis\" to a new drive letter like "V:\".

The %~d0 in the batch expands to the drive letter of the full path name of the batch.

Executing this batch should then create a new Drive Letter V containing all the Data of your Portable. When creating projects, just save them in V:\MyProjectBlah\ instead of P:\MyProjectBlah\ (where P:\ shall be the drive letter Windows 'knew' you were about to choose when plugging in the device)

before removing the Portable you should run (maybe in a batch?)

```bat
subst V: /D
```

which drops the substitution for drive letter V:\

This should generally solve the problem of changing drive letters except for the fact that your desired drive letter (V:\ in example) might already be in use. "B:\" should work on most computers. 