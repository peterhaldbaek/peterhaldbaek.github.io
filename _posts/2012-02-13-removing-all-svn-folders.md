---
title: Removing all .svn folders
layout: post
tags: ["linux","subversion"]
date: 2012-02-13
commentsUrl: http://www.briskbee.com/2012/02/removing-all-svn-folders.html#comment-form
redirect_from: "/posts/removing_all__svn_folders/"
---

Every now and then I need to copy a folder which is under source control. By copying the folder all source control information is copied along which needs to be removed afterwards. If the amount of folders is small this is done pretty easy manually but when we are talking larger amounts of folders doing it manually easily ends up pretty time consuming.

I figured there would be a pretty easy solution for this with some nifty Linux command line magic and it didn't take long to find [something](http://www.thelinuxblog.com/remove-all-subversion-svn-folders/) that worked:



```
> find . -iname ".svn" | xargs rm -r $1
```

This removes all .svn folders recursively.
