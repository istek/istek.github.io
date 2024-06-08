---
id: 1164
title: Fix mbr record in windows xp
date: 2009-08-27T20:04:18+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1164
permalink: /fix-mbr-record-in-windows-xp/
categories:
  - 我的世界
tags:
  - fixmbr
  - windows
---
Yesterday i reinstalled my ubuntu, but i don’t know how to destory my mbr record. I’ve 2 hard drives, one is 120G IDE hard drive installed Ubuntu, the other is 250G SATA harddrive installed Window Xp. And usually 120G harddrive is my first boot disk, and grub should be installed there. But last night i installed Grub2, I don’t know how to rewrite my 250g disk’s mbr.

Okay,right now let me fix it. You need administrative privileges to make it work!

First,you need a tool named “MBR Fix”, [click here](http://rapidshare.com/files/272003046/mbrfix.zip.html) to download.

Second, extract it to a directory. You can find there’s only one command line tool.

Third, open your dos prompt, cd to the directory where your mbrfix locates.

Forth, use this grammer to fix your mbr. 
   
class=”brush: bash”>MbrFix /drive <num> fixmbr {/vista|/win7} Update MBR code to W2K/XP/2003, Vista or Win7

And here below is all available command: 

> MbrFix /drive <num> driveinfo Display drive information 
     
> MbrFix /drive <num> drivesize Returns drive size in MB as return value 
     
> MbrFix /drive <num> listpartitions Display partition information 
     
> MbrFix /drive <num> savembr <file> Save MBR and partitions to file 
     
> MbrFix /drive <num> restorembr <file> Restore MBR and partitions from file 
     
> MbrFix /drive <num> fixmbr {/vista|/win7} Update MBR code to W2K/XP/2003, Vista or Win7 
     
> MbrFix /drive <num> clean Delete all partitions on the selected disk 
     
> MbrFix /drive <num> readsignature {/byte} Read disk signature from MBR 
     
> MbrFix /drive <num> writesignature <hex> Write disk signature to MBR 
     
> MbrFix /drive <num> generatesignature Generate disk signature in MBR 
     
> MbrFix /drive <num> readstate Read state from byte 0x1b2 in MBR 
     
> MbrFix /drive <num> writestate <state> Write state to byte 0x1b2 in MBR 
     
> MbrFix /drive <num> readdrive <startsector> <sectorcount> <file> 
     
> Save sectors from drive to file 
     
> MbrFix /drive <num> /partition <part> fixbootsector <os> 
     
> Update Boot code in boot sector 
     
> MbrFix /drive <num> /partition <part> getpartitiontype 
     
> Get partition type 
     
> MbrFix /drive <num> /partition <part> setpartitiontype <typenum> 
     
> Set partition type 
     
> MbrFix /drive <num> /partition <part> setactivepartition 
     
> Set active partition 
     
> MbrFix /drive <num> getactivepartition Get active partition 
     
> MbrFix volumeinformation driveletter Get volume information for partition 
     
> MbrFix flush {driveletter(s)} Flush files to disk for partition 
     
> MbrFix listpartitiontypes List partition types