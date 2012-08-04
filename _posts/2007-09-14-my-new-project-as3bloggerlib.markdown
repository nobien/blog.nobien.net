---
comments: true
date: 2007-09-14 07:40:11
layout: post
slug: my-new-project-as3bloggerlib
title: 'My new project: AS3BloggerLib'
wordpress_id: 31
categories:
- actionscript 3
- air
- flex
---

Ah, actionscript libraries. You can never have [too](http://code.google.com/p/as3syndicationlib/) [many](http://code.google.com/p/as3flickrlib/) of [them](http://code.google.com/p/as3corelib/), right? Right. And not to mention that there's nothing better than someone writing code for you. In all honesty I haven't had much time to myself to use any of the wonderful AS3 libraries that are out there right now, but while working on a chapter for [AIR Instant Results](http://www.amazon.com/AIR-Instant-Results-Marc-Luchner/dp/0470182075/) I was inspired to start my own project.

The chapter utilizes the [Blogger Data API](http://code.google.com/apis/blogger/overview.html), but because it must be compressed into 30 or so pages, I have to develop a very simplified set of classes to handle the communication. Any OOP stickler would probably give me a hard time about the class structure, but thats a compromise that you sometimes have to make when writing a book. Yes, because I love OOP programming and can't get enough of it, it's certainly sad to have to make compromises. However, this hasn't stopped me from reconciling my own guilt. In my spare time I've been diligently working on a comprehensive AS3 library for the Blogger API. Hooray! Guilt-be-gone.

[AS3BloggerLib](http://code.google.com/p/as3bloggerlib/) is my first leap into creating my own open source Actionscript project, and I'm looking forward to fixing, updating, changing, and redesigning this thing until its picture perfect. In all honesty, it still needs some sprucing up (needs lots of encapsulation), but works pretty well for the most part. Certainly there are things that are missing, most notably documentation, but these things should appear in the near future.

No packaged downloads are available yet, so if you're interested in getting at the code you'll have to do an SVN checkout to get the source. Eventually there will be a packaged SWC, examples, and solid documentation.  Also, I certainly am not opposed to any criticism or suggestions for improvement, so don't hesitate to start discussing things on the [discussion group](http://groups.google.com/group/as3bloggerlib).

The last note I should make is that this library only works for installed (AIR) applications at the moment because Google does not host a crossdomain file. Other than that, I hope this goes over well!
