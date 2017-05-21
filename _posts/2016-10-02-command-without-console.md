---
layout: post
title: Run Command Without Console
tags: [vb]
---

This VBScript allows you to run a command without a console popping open.

```
CreateObject("Wscript.Shell").Run "COMMAND PARAM1 PARAM2", 0, True
```
