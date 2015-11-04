---
layout: post
title: Useful tools
tags: [windows,registry]
---

Some useful command prompt tools for Windows 2000 by editing the registry. Adds a "Command Prompt" to the right click menu in explorer for current folder. Also adds tab completion and history to the command prompt. Save these as .reg files and double click.

To install:

```
Windows Registry Editor Version 5.00
 
[HKEY_CLASSES_ROOT\Folder\shell\CmdHere]
@="Command &Prompt"
[HKEY_CLASSES_ROOT\Folder\shell\CmdHere\command]
@="cmd.exe /k \"cd %L\""
 
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor]
"CompletionChar"=dword:00000009
"PathCompletionChar"=dword:00000009
```

To uninstall:

```
Windows Registry Editor Version 5.00
 
[-HKEY_CLASSES_ROOT\Folder\shell\CmdHere]
 
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor]
"CompletionChar"=dword:00000040
"PathCompletionChar"=dword:00000040
```
