---
layout: post
title: Ping sweep
tags: [bash,python]
---

Ping a set of hosts on a network to see which are online.

In bash:

```bash
#! /bin/bash
BASE=$1
START=$2
END=$3

counter=$START

while [ $counter -le $END ]
do
  ip=$BASE.$counter
  if ping -c 1 -t 1 $ip > /dev/null 2> /dev/null
  then
    echo "$ip responds"
  fi
  counter=$(( $counter + 1 ))
done
```

In python using threads:

```python
import os
import re
import time
import sys
from threading import Thread

class testit(Thread):
   def __init__ (self,ip):
      Thread.__init__(self)
      self.ip = ip
      self.status = -1
   def run(self):
      pingaling = os.popen("ping -q -c2 "+self.ip,"r")
      while 1:
        line = pingaling.readline()
        if not line: break
        igot = re.findall(testit.lifeline,line)
        if igot:
           self.status = int(igot[0])

testit.lifeline = re.compile(r"(\d) packets received")
report = ("No response","Partial Response","Alive")

try:
  baseip = sys.argv[1]
  startip = int(sys.argv[2])
  endip = int(sys.argv[3])
except Exception:
  print ("SYNTAX: python "+sys.argv[0]+" base start end")
  exit(1)

print (time.ctime())

pinglist = []

for host in range(startip,endip+1):
   ip = baseip+"."+str(host)
   current = testit(ip)
   pinglist.append(current)
   current.start()

for pingle in pinglist:
   pingle.join()
   print ("Status from ",pingle.ip,"is",report[pingle.status])

print (time.ctime())
```
