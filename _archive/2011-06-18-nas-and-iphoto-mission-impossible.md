---
title: NAS and iPhoto, Mission Impossible?
layout: post
tags: ["nas","mac","iphoto"]
date: 2011-06-18
redirect_from: "/posts/nas_and_iphoto__mission_impossible_/"
published: false
---

Before I got my MacBook Pro I used my old Mac Mini with Tiger installed as my primary storage for my photos (using iPhoto). I then purchased a NAS, formatted it with ext3, copied my iPhoto library to the NAS and configured my Mac Mini to use the NAS as iPhoto library instead of the library stored on the Mini itself. It worked like a charm and now my photos would also be available to my other devices on my network (phones, other computers, media renderers etc.).

![](/assets/img/iphoto.png)

My troubles started when I added a MacBook Pro running Snow Leopard to the equation. I now wanted to use this computer as the one maintaining my photo library. At first I guessed I could use the same approach as I had done with my Mac Mini. This quickly turned out to be impossible since

1.  iPhoto libraries for Tiger and Snow Leopard are not compatible
2.  And apparently something happened with iPhoto in later versions since it is not possible to put the iPhoto library on an ext3-formatted disk.Formatting the NAS with something else is not a solution for me since I want to be able to support Macs, Linux and Windows with my NAS. It is worth noting that even if it was possible I believe iPhoto would become extremely slow if the photos were stored on the network (I only noticed this for the Snow Leopard version, the Tiger version did not seem to be affected by this).

So I decided to look for a solution where:

*   My photo library should be stored on my NAS
*   The NAS is formatted with ext3
*   The folder structure of the library should be configurable. I specifically wanted to store my photos in a structure like this: year / month / date.
*   The export of photos from camera to computer should be done automatically (I do not want to drag images into correct folders by hand)
I figured this could be achieved in two ways. Either by finding a way of exporting my photos directly to the NAS and then later importing these photos into iPhoto (I actually do like iPhoto I just hate the lousy support for network storage) or letting iPhoto import the photos to my MacBook Pro and then schedule a job to automatically copy the files to the NAS. I do not need to sync the Mac Mini since it is more or less retired.

**Importing photos to NAS**
I first explored the possibilities given by Apple.

**Image Capture**
Mac OS X comes with a little utility called Image Capture which exports photos from your camera to your computer. I never explored whether it supported ext3 formatted disks since you have to export the photos to your destination manually. Image Capture does support automatic export but then all your exported images will end up in the same folder. So I quickly turned to other solutions.

**Autoimporter**
A (very) secret photo utility in Mac OS X is called Autoimporter. It is not accessible through the Applications menu so you will have to open Finder and locate the application by hand. It can be found at /System/Library/Image Capture/Support/Application/AutoImporter.app.

[![](/assets/img/screen_2bshot_2b2011-06-18_2bat_2b22_51_14.png)](/assets/img/screen_2bshot_2b2011-06-18_2bat_2b22_51_14.png)

At first this tool looked very promising. It basically (as it name suggests) imports photos automatically based on your configuration. It accepts parameters such as date, camera name, user name and sequence number. Unfortunately the date is the date of the import not the date when the picture was taken and it does not support nested folders so I turned to options offered by 3rd parties.

**Kodak Easy Share**
Kodak has released a free photo library tool called [Kodak Easy Share](http://www.kodak.com/ek/US/en/Consumer_Products/Organize_Print_Share/KODAK_EASYSHARE_Software_17.htm?pq-path=130&amp;pq-locale=en_US&amp;_requestid=19651). It was really hard finding the downloadable application (and I forgot where I found it) but do not bother. It stinks. It looks horrible on a Mac and the export functionality did not support nested, configurable folders so this was not an option either.

**digiKam**
I was a little excited to try this out since it has a very good reputation. It is open-source and runs on both Linux, Windows and Mac so I was really hoping this could solve my problems. It is not supported on Mac out of the box though and I had lots of problems intalling it but I succeeded in the end. Follow the instructions at [http://www.digikam.org](http://www.digikam.org/), pray and wait (it took me 2 full days for everything to compile).

[![](/assets/img/digikam.png)](/assets/img/digikam.png)

I really liked digiKam as a product. It seems very polished (not as polished as iPhoto) considering it is open-source and there were tons of options. The import from camera functionality however suffered from the same limitations as I discovered with the other tools. No support for nested, configurable folders so once more it was back to the drawing board. I was very impressed though and would seriously consider digiKam over F-Spot next time I use Linux.

**Using iPhoto and rsync**
That was it. I gave up on finding software which could copy photos from my camera to a folder structure on my NAS ordered by year, month and date. A task so seemingly simple but apparently impossible to achieve. Instead I opted for option number two. Importing photos to my laptop using iPhoto and then synchronize the NAS with the contents on my laptop.

I figured I could use rsync for that but I had some problems locating my photos in the iPhoto library. I did find some photos but I was not sure whether they were the originals or not so I decided to search Google for advice. I quickly realized that messing up with the internals of the iPhoto Library (even if it is only copying) is not the way to go. The later versions of iPhoto (pre 9 I think) started storing images in a database instead of the file system making it hard to just copy the photos to my NAS.

It was about then my first revelation came. Either I use iPhoto as I am supposed to (as Steve wants me to that is) or I skip it. Messing with the internals of iPhoto would only give me headaches. I decided to continue using iPhoto but my problem was still not solved.

**Phoshare to the rescue**
Months went by and I tried to convince myself I was happy with iPhoto as it was. And then I accidently came across a neat little utility called [Phoshare](https://sites.google.com/site/phosharedoc/). Phoshare lets you export photos from iPhoto (almost) exactly as you want. It does not support nested date folders but it probably supports everything else you could wish for. It also exports image metadata and it is pretty fast too.

[![](/assets/img/36036.png)](/assets/img/36036.png)

So at last I could breath a sigh of relief (and satisfaction). Now I use my iPhoto installation on my laptop as my main administration tool and for import of photos. After each import I run Phoshare and it synchronizes my laptop and NAS. I do not have nested date folders but I started using the iPhotos event functionality and order my photos in folders with year and event name. It makes it pretty easy to navigate from my other devices at home and it works like a charm.

Finally I am done. I cannot wait to see if iMovie is up to the task of sharing my movies through my NAS (slight sarcasm might have been used here).
