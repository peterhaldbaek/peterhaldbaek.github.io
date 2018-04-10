---
title: Secret menu for Huawei u8860
layout: post
tags: ['huawei','android']
date: 2014-03-11
redirect_from: "/posts/secret_menu_huawei_u8860/"
---

I have an old [Huawei 8860](http://en.wikipedia.org/wiki/Huawei_u8860) Android phone which keeps running out of internal memory space. I constantly uninstall apps to make room not only for new apps but also just to be able to update my existing apps despite having more than 2GBs available according to the Storage settings. It turns out (I assume) that this behavior is caused by logs hiding in the <code>/data/log</code> folder. Too bad it is not possible to delete the files unless the device is rooted which I have not done (yet). I am still hoping to figure something out and recently found out that there is a secret system menu if you dial <code>\*#\*#2846579#\*#\*</code> in the stock phone app. It did not help me with my problem though.

## Update 22.04.2014
After repeated searches for answers on the web about my internal memory issues with my Huawei came up fruitless I went ahead and rooted the device. Turns out there is no hidden <code>/data/log</code> folder (or any other folder containing garbage for that matter). I did find out that cleaning the <code>/data/tombstones</code> folder would give me (tiny) space improvements but overall the findings were very disapointing.

My next suspicion was looking a little bit closer at the storage required by the mandatory Google apps. It did seem like they were taking up a lot of space and since they cannot be moved to your SD card they are just hogging up internal memory. I figured a factory reset of my phone would reveal more. After the factory reset (but before updating all the mandatory apps) my phone had more than 600MB of internal memory (the phone is shipped with 1GB internal memory so I assume the rest is used for the OS). I updated all the mandatory Google apps and internal memory went down to 350MB! Experience has shown me that when the phone comes close to 100MB of free internal memory it starts getting cranky and _really_ slow so this leaves me with 250MB of free space. Not really encouraging since all the mandatory apps like Facebook, Twitter, Google+, Gmail, Chrome, Hangouts and Maps caches data in internal memory so it is only a matter of (short) time before I am back to square one. Did I mention that the apps are mandatory despite I am not using apps like Google+ or Hangouts and possibly others as well?

I understand that older phones over time are considered inferior to newer models but I do not expect older phones to become useless all of a sudden as is the case right now. I believe there is a huge market for budget, low-spec smartphones (especially in 3rd World countries) but if the OS keeps requiring us to install bigger and bigger mandatory apps from Google I think they are making a mistake.