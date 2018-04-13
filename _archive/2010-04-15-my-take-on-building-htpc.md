---
title: My take on building a HTPC
layout: post
tags: ["htpc","mythtv","mac"]
date: 2010-04-15
commentsUrl: http://www.briskbee.com/2010/04/my-take-on-building-htpc.html#comment-form
redirect_from: "/posts/my_take_on_building_a_htpc/"
published: false
---

After months (years?) of tinkering with my HTPC build I am finally at a point where I can say that I am done. There is always a lot of extra things to do with such a versatile box but my main goal was to build my own DVR and that is where I am at now. If I ever want to build such a system again (not sure about that) this is what I would consider:

## Mac Mini

While I was building the HTPC I decided to buy a used [Mac Mini](http://en.wikipedia.org/wiki/Mac_mini) just for the fun of it. I bought the first generation Mac Mini which comes with a 1.42 GHz PowerPC processor and ATI Radeon 9200 graphics card. I wanted to test out programming on the Mac platform (haven't done that yet), maybe try it as a frontend to the HTPC I built (haven't done that yet) or video editing for my digital camcorder (tried it, works perfectly). I was blown away with the thing on more levels and I can't believe I haven't paid more attentions to Macs before (the price tag is probably a good explanation).
Here is what I really like about the Mac Mini platform:

* It looks great (100% [WAF](http://en.wikipedia.org/wiki/Woman_acceptance_factor) proof (if not more!))
* Really silent (my Mac Mini does have a fan but it is very silent).
* Power efficent (when idle I could merely measure any draw, in use 25W)

I could easily imagine using the Mac Mini as both frontend and backend because of the low power use and sleek design. My own HTPC box runs at 60W when in use but I never really got any luck with different modes of suspend (it is a real pain to setup in Linux) so I could not test idle modes in my HTPC build.
The Mac Mini can be equipped with a TV tuner from for instance [Elgato](http://www.elgato.com/) and then you should be ready to go. The drawbacks of the Mac Mini are:

* Hard to change hard disk or memory as the case is not assembled with screws. It can be done though, take a look [here](http://www.youtube.com/watch?v=ynQKYTaJ_zA&amp;feature=player_embedded#%21).
* Older models (1st generation) comes without infra red controller. If you want a remote control for one of these models you need to buy a USB controller for this.
* Pricey

## Distributions

I ended up trying out three different distributions of MythTV:

* [KnoppMyth/LinHES](http://www.linhes.org/)
* [Mythbuntu](http://www.mythbuntu.org/)
* [MythDora](http://www.mythdora.com/)

I ended up using Mythbuntu (9.10) since they were the first to support MythTV 0.22 (important to me since I wanted to use VDPAU). I do not regret this decision since Mythbuntu has a large following and lots of committed developers. I was used to Ubuntu beforehand which made the decision easier. I did however like LinHES a lot since this distribution seems to be more polished than the other two. Everything seems more tested and ready than I experienced with Mythbuntu but Mythbuntu won in the end since I was more familiar with Ubuntu and I found it easier to setup my remote control with this distribution. I never gave MythDora a fair shot and just installed it out of curiosity but since it did not solve any of the problems I had initially with the two other distributions (getting my Hauppauge to work satisfactory) I uninstalled it again. One thing that worried me about LinHES was that it was handled by one person alone. I wonder what would happen to the distribution if he decided to do something else.

## Conclusion

So if I ever was to build a HTPC again I would probably pick up a Mac Mini (one of the newer with nVidia graphics) and equip it with a TV tuner and remote control. I would run Leopard or Snow Leopard as OS and install [MythTV](http://www.mythtv.org/wiki/Installing_MythTV_on_an_Intel_Mac_Mini_using_Ubuntu) on top of that. I would also install [Plex](http://www.plexapp.com/) which looks really nice. I haven't been able to test it myself since I only run Tiger on my Mac Mini.

## Update June 30th 2010

After changing the hard drive of the Mac I realized it actually has a fan! Sorry for the confusion. It is still pretty silent though.
