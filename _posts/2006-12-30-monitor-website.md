---
layout: post
title: Monitor website
tags: [web-scraping,linux,bash]
---

This script monitors amazon.com for the wii

```bash
while true;
do wget -q -O - http://www.Amazon.com/exec/obidos/ASIN/B0009VXBAQ/ | grep -10 -f /tmp/wiit |grep -v '^$' > /tmp/awii;
grep Amazon /tmp/awii;
if [ $? = 0 ];
then mailx -s wii pager#@yourpager.com < /tmp/awii;
fi;
sleep 10;
done
```

my /tmp/wiit file contains:
Availability
In Stock
