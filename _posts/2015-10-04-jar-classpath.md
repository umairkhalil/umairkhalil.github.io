---
layout: post
title: Jar classpath
tags: [java,batch]
---

When running a java program that needs .jar files, you need to add them to the classpath. If the jar files are spread in different subfolders you have to specify them individually because the wildcard \* isn't recursive. This script allows you to add multiple jar files in different subfolders to the classpath without specifying them individually.

Windows batch file:

```bat
@echo off

REM This file allows you to add multiple jar library files in subfolders to the classpath
REM This won't work with spaces in file or folder names
REM Usage: ThisFile PathToLibraryFolder

FOR /R "%~1" %%a in (*.jar) DO CALL :AddToPath %%a
ECHO %CLASSPATH%
GOTO :EOF

:AddToPath
SET CLASSPATH=%1;%CLASSPATH%
GOTO :EOF
```
