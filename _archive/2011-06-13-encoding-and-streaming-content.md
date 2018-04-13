---
title: Encoding and streaming content
layout: post
tags: ["htpc"]
date: 2011-06-13
published: false
---

I recently bought a TV screen capable of playing movie streams via DLNA. Since my HTPC contains lots of recorded TV shows I wanted to make these available to the rest of the network. To do so I looked into various movie formats and how to encode content into these formats making them available to my new TV screen.

I wanted to encode my TV shows into MPEG-4 and quickly discovered that this format comes in lots of different flavors. Most notably is the distinction between [MPEG-PS](http://en.wikipedia.org/wiki/MPEG-PS) (Program Stream) and [MPEG-TS](http://en.wikipedia.org/wiki/MPEG-TS) (Transport Stream). MPEG-PS is mostly used as encoding when the media is stored on DVDs where the transport of the data is reliable whereas MPEG-TS is used when streaming content via terrestrial or satellite broadcast.

I created two scripts, one for MPEG-PS and one for MPEG-TS. Both scripts use the command line version vlc.

```
cvlc "$1" vlc://quit --sout="#transcode{vcodec=mp2v, vb=1024, acodec=mpga, ab=128}:standard{mux=ps, dst='$2', access=file}"
```

```
cvlc "$1" vlc://quit --sout="#transcode{vcodec=h264, vb=1024, acodec=mpga, ab=128}:standard{mux=ts, dst='$2', access=file}"
```

Since I stream the media from my NAS I decided to use MPEG-TS when encoding the shows into something my TV would understand.
