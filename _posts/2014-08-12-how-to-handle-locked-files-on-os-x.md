---
layout: post
title: "How to handle locked files on OS X"
modified: 2014-08-12 22:35:39 +0300
tags: [OS X]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

It's not uncommon for a file to get locked by some process. In that case if you try to delete it you might get a messagebox like this - *"The operation can't be completed because the item ... is in use."*

![]({{ site.url }}/images/figures/locked-file.png)

To find out which process locks the file just open the Terminal.app and type `lsof | awk 'NR==1 || /PartOfFileName/'`

![]({{ site.url }}/images/figures/locked-file-terminal.png)

The first column contains the name of the process that locks the file and the second is the PID (Process ID). You can then close the app that locks the file or fire up Activity Monitor and kill the process.

Hope this helps