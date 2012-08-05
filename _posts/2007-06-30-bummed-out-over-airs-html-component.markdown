---
comments: true
date: 2007-06-30 17:58:21
layout: post
slug: bummed-out-over-airs-html-component
title: Bummed out over AIR's HTML component
wordpress_id: 24
categories:
- air
- flex
---

For perhaps obvious reasons, I've been trying to make a web browser using AIR's HTML component. I made some really quick assumptions about how this component was going to work and how much functionality Adobe would be putting into it. My assumptions are partly based on an entry in the [AIR Developer FAQ](http://labs.adobe.com/wiki/index.php/Apollo:DeveloperFAQ). It reads:

** Q: Is Adobe AIR a web browser?**
_ A: No. Adobe AIR is a cross-operating system runtime that runs outside of the browser. Theoretically you could build a web browser on top of Adobe AIR.
_
Naturally I thought: "Wow, what a sweet possibility!" I could make a cool little custom browser with some rad built in features. But it wasn't until after hours and hours of trying to create a general purpose browser, I've come to a breaking point. I should have realized this earlier, but its just not made for general web browsing yet. I mean, a quick Google search reveals that no one else out there seems to be trying to make a browser on top of AIR. Not to mention you can't open links that use an "_blank" target or open Javascript created windows.

It's my own fault really for making the assumption this was possible. So now I can only hope that we get something more robust in the future.
