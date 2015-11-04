---
layout: post
title: Useful Vim commands
tags: [vim]
---

Some useful Vim commands.

```vim
"aYj"bY2j"ap2j"bp
```

(" (Store next-command in buffer) a, Yank-line. j Move to next line. " (buffer) name b Yank line. (Repeat next command) 2 times - j line-down. Then from " (buffer) name a paste. 2 lines j (down), From " (buffer) b, paste.).

```vim
10Gma2jd'a
```

Goto line 10, mark line/col-number to buffer a, move down j 2 lines. d from current line to marked line a

```vim
02wma2j01wd`a
```

Go to 0'th column of current line. Jump 2 words forward. mark line/col to buffer a. Jump 2 lines j (down). Go to 0'th column. Jump 1 w word forward. delete from the `current line/col to the line col in buffer.
