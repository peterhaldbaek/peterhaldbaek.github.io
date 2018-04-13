---
title: MythTV setup
layout: post
tags: ["htpc","mythtv"]
date: 2009-03-14
commentsUrl: http://www.briskbee.com/2009/03/mythtv-setup.html#comment-form
redirect_from: "/posts/mythtv_setup/"
published: false
---

I have considered building my own media center for a long time and have finally had the time to look into the hardware requirements for running [MythTV](http://www.mythtv.org/). Originally I wanted to build around a mini-ITX motherboard but most of them lacked a lot in performance since I wanted my system to be able to play and record full HD (1080p). Most mini-ITX cases I looked at were really cool because of their tiny size but had on the other hand some serious issues when it came to cooling so I ended up looking at micro-ATX motherboards instead. I also wanted the system to be power efficent so there has to be a balance between power utilization and efficiency.

Here is what I have come up with so far for a combined backend/frontend:

## Motherboard - Asus M3N78-VM

* Pros: nVidia 8200 chipset which supports VDPAU, HDMI.
* Cons: no TV-out, micro-ATX (not really an issue I just really wanted to go the mini-ITX way but have not found a motherboard I really liked yet, the Jetway JNC62K could be an option though), no support for undervolting in BIOS

## CPU - AMD Athlon X2 4850e

* Pros: Fast, cheap, power efficent considering the speed.
* Cons: Maybe a little overkill for running MythTV but I want the machine to be able to emulate games (C64, Amiga 500, Mame, PSX) so I guess some processing power does not hurt.

## Memory - Kingston HyperX 2GB DDR2 1066

* Pros: Cheap.
* Cons: I could probably use memory with lower speed.

## HDD - Samsung SpinPoint F1 HD753LJ 750GB

* Pros: Low noise.
* Cons: 2.5" hard drives are more quiet (but also more expensive).

## TV tuner - Hauppauge WinTV Nova-T-500 PCI

* Pros: Dual tuner card, supports DVB-T (digital), IR remote control included, cheap.
* Cons: Unclear if it supports MPEG-4.

## DVD burner - Pioneer DVR 216DBK

* Pros: Cheap, relatively low noise.
* Cons: Not that silent but few DVD players are (if any at all).

## Case - Antec NSK2480

* Pros: Stylish design, cheap, effective cooling, PSU included.
* Cons: Big, no VFD, no IR.

## PSU - Sparkle Power FSP200-50SNV

* Pros: Cheap, low noise
* Cons: Not 80 Plus

All in all this system will cost me around DKK 3700 (~EUR 500) which is not that bad considering how stylish the system is and how much functionality it offers.
