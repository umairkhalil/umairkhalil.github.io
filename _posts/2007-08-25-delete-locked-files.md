---
layout: post
title: Delete locked files
tags: [windows]
---

On windows, if you are unable to delete files for some reason, try the following to get a prompt using the system account and then you can delete files. So if it is 9:01pm type:

```powershell
at 9:02pm /interactive %systemroot%\system32\cmd.exe
```

and at 9:02pm a prompt will open that will be running as system.
