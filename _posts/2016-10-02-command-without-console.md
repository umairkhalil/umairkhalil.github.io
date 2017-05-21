---
layout: post
title: Run command without console
tags: [vb]
---

This VBScript allows you to run a command without a console popping open.

```
CreateObject("Wscript.Shell").Run "COMMAND PARAM1 PARAM2", 0, True
```
