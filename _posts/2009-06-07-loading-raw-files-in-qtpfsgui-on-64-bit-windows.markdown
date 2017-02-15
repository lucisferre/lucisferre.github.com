---
title: Loading RAW files in Qtpfsgui on 64-bit windows
date: 2009-06-07 20:16:00 Z
categories:
- photography
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '13'
---

When I first downloaded Qtpfsgui for creating HDR images I had a problem where I could not load my camera's raw format images (Pentax PEF). Qtpfsgui uses dcraw for loading RAW images and whenever it tried to load the PEF files Qtpfsgui spit out a misleading error that led me to believe that perhaps dcraw didn't support PEF files. This was not the case, running dcraw directly from the command line gave me the answer I needed: 

`This version of C:\Program Files (x86)\Qtpfsgui\dcraw.exe is not compatible with the version of Windows you're running. Check your computer's system information to see whether you need a x86 (32-bit) or x64 (64-bit) version of the program, and then contact the software publisher.`

I solved this by downloading the DcrawMS.exe from [here][1] which works under 64-bit windows. I put it in the Qtpfsgui folder, deleted dcraw.exe and renamed dcrawMS.exe -> dcraw.exe so that Qtpfsgui would find the new version. Worked like a charm, however the HDR wizard doesn't show a preview when importing PEF files. Oh well, at least it is working for now. 

If anyone else is trying to use Qtpfsgui under 64-bit Vista or Windows 7 I hope this is helpful.

   [1]: http://www.insflug.org/raw/Downloads/

