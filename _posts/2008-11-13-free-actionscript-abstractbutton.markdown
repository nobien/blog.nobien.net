---
comments: true
date: 2008-11-13 12:54:12
layout: post
slug: free-actionscript-abstractbutton
title: Free ActionScript! AbstractButton
wordpress_id: 86
categories:
- actionscript-3
- flash
---

As many of you might know, when you're doing Flash everyday for a job, you find yourself doing a lot of the same things over and over. Such things include building a navigation system, a video player, a form, etc. At some point you say to yourself, there's got to be a way to improve my productivity with this. If I remember correctly, the first time I asked myself this I had no idea where to start. But, a couple days later it finally hit me. Probably the most common task I have when building flash content is making buttons.

Now, the kinds of designs I'm usually provided with at work are usually pretty graphic and the designers like fancy rollovers that the SimpleButton class just can't provide. Because of this I often use MovieClips as buttons and place assets within that MovieClip to animate and/or modify. So what I end up doing is creating MovieClips in the IDE and attaching a class to that MovieClip. Pretty standard.

So then I started to think, how can I make a class that is sorta like SimpleButton but gives me the flexibility I need for my "fancy" rollovers? Now, I'm no OOP wizard by any means, but it was then that I stumbled across what are called Abstract classes. Abstract classes are meant only to be extended and provide a code base for your more specific class. So It made sense to me to create an AbstractButton class that is always meant to be extended and would contain basic button functionality but remain ingorant to any animation or child objects needed for any effects.

I started experimenting and analyzing most of my previously made button classes and started to see some patterns in the basic button functionality. In the end I discovered my most used events and most used methods and added them to my AbstractButton class. I'm pretty happy with this class and it makes making buttons so much faster than it ever did before, especially with ActionScript 3. All I have to do now is create a class, extend AbstractButton and, in most cases, just override the onRollOver and onRollOut functions. Depending if the button is part of a nav system, I'll override the select() and deselect() methods as well. If the button just simply links to a URL, all I have to do is override the onClick method and add a navigateToUrl() call.

So in hopes of helping others, I'd like to share my class with all of you. I would certainly love to hear any comments, suggestions, or questions regarding the class.

[Download the AbstractButton class](http://blog.nobien.net/actionscript/AbstractButton.zip).
