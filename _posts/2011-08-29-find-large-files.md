---
layout: post
title: Find large files
tags: [linux,bash]
---

This is a script to build a table of large files, sorted by size and insert it into a text file:

```bash
find /home -type f -size +2G -exec ls -lh {} \; | cut -d" " -f5,8 | sort -t" " | tee ~/largefiles.txt 
```
