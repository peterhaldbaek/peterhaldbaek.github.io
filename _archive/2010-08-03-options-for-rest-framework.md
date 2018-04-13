---
title: Options for  REST framework
layout: post
tags: ["java","rest","google","android"]
date: 2010-08-03
commentsUrl: http://www.briskbee.com/2010/08/options-for-rest-framework.html#comment-form
redirect_from: "/posts/options_for__rest_framework/"
published: false
---

Developing applications for the mobile platform is not only about creating code for the iPhone or Android but just as much about leveraging the possibilities of cloud computing. The mobile applications need to take advantage of the information and processing power within the cloud. I have been looking for ways of implementing cloud services in the most efficient manner but have not come up with the holy grail yet. I need a lightweight service layer exposing its services as REST services. Until now I considered using [Rails](http://rubyonrails.org/) for this (I still do) but after I started playing around with the Android framework I thought the [Google App Engine](http://code.google.com/appengine/) maybe could offer something.

![](/assets/img/gaerestlet.jpg)

Today I discovered that [Restlet](http://www.restlet.org/) integrates with GAE so that is another option worth exploring.
