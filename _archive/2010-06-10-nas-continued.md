---
title: NAS continued
layout: post
tags: ["python","nas","mac"]
date: 2010-06-10
commentsUrl: http://www.briskbee.com/2010/06/nas-continued.html#comment-form
redirect_from: "/posts/nas_continued/"
published: false
---

I recently did a little writeup on some NAS products in the budget price range. I finally decided to buy a ReadyNAS RND2000 with 2 1 TB Seagate harddrives but bailed out in the last minute only to reconsider why I needed a NAS. I basically just wanted to have a common fileserver for all my computers at home and I wanted it to be really power efficient. I believe the ReadyNAS server is pretty power efficient but I have an old Mac Mini G4 in the basement which does not do anything at the moment. I remember I was pretty excited about the power consumption of the Mac when I first got it (20W work, close to nothing when idle) so I have decided to turn the Mac into my NAS/fileserver. I do not need RAID just a proper backup plan.

![](/assets/img/Mac_mini_Intel_Core_transparent.png)

So now I am in the process of upgrading the hardware of the Mac. I am upgrading the memory from 512Mb to 1Gb and upgrading the harddrive from 80Gb to 320 Gb. A sideeffect to all this is that I can get rid of my old noisy Ubuntu box in the basement as well which I use as a regular desktop computer. The Mini will handle that from now on. I really do not know why I never thought of this before.

The only concern I have with the Mac Mini now is that I want it to go to sleep when it is not in use and wake up when someone tries to access data stored on it. My first experience with this was that when it first went into sleep mode I had to press a key on the keyboard of the Mac itself, which is not very convenient when it is in the basement. I could just configure it to not to go into sleep mode at all but that kind of defies the purpose of using it as a (power-efficient) NAS.

I stumbled across this little tip on the net. If you execute this Python script on any computer trying to access the Mac it should wake up from its sleep mode. The MAC address of the Mac (no pun intended) needs to be specified in the scripts (in this case it is set to 00 11 22 33 44 55).

```
#!/usr/bin/env python

import socket;

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM);
s.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1);

s.sendto('\xff'*6+'\x00\x11\x22\x33\x44\x55'*16, ('192.168.0.255', 9));
```

The only problem with this is that you have to run the script when you want to access something on the Mac. I am planning to put all my music on the Mac and then access it from devices which may not be able to execute such a script (like the Logitech Squeezebox Duet). I have not quite figured out how to solve this yet but since it is not a problem right now (I do not own a Squeezebox) I will take care of this later. The good thing with this script is that you can run it on any platform supporting Python. I have successfully tested it with both Linux (Ubuntu 8.04) and Windows XP.
