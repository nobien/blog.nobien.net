---
comments: true
date: 2007-11-01 13:20:12
layout: post
slug: flash-embed-tip-minimum-widthheight-for-liquid-layouts
title: 'Flash Embed Tip: Minimum width and height for liquid layouts'
wordpress_id: 41
categories:
- flash
- javascript
---

**Update:** This post is old, and getting a decent amount of traffic, so I've decided to provide a link to a more recent post that handles this situation much better! [Just One of My Favorite Flash Tools: swffit](http://blog.nobien.net/2008/11/17/just-one-of-my-favorite-flash-tools-swffit/)

Over the course of your career you often come up with solutions to such small problems that you completely forget about them. One instance for me is coming with a really simple way to ensure that a Flash site has a minimum height and/or width while using a liquid layout. When a designer gives you a comp and he or she says "I want this to be 'fullscreen'", you immediately think that you'll be setting the width and height of the embed code to 100%. However, you soon realize that you have to account for smaller screen sizes than the size at which the site was designed for.

You could create you're own horizontal and vertical scrollbars within your Flash piece to handle such a situation, but I much prefer the scrollbars within the browser, as a user is much more apt to recognize them and feel more comfortable using them. Here is my tip for handling this.

Say you have a piece of Flash content embedded in your page with SWFObject as such:

    
    var so = new SWFObject("Home.swf", "myFlashContent", "100%", "100%", "8", "#000000");
    so.write("flashcontent");


Straightforward, of course. Now start by adding this Javascript function up within the page's HEAD tag:

    
    function adjustSWFHeight() {
        if(document.body.clientHeight <= 768)
            document.getElementById("myFlashContent").style.height = "680px";
        else
            document.getElementById("myFlashContent").style.height = "100%";
    
        if(document.body.clientWidth <= 1000)
            document.getElementById("myFlashContent").style.width= "1000px";
        else
            document.getElementById("myFlashContent").style.width= "100%";
    }


Simple enough, right? The only thing you'll have to customize is the condition statement which defines the minimum height of the Flash content and the edge at which to use the minimum height over a 100% value. Next, set the onLoad and onResize properties of the page's BODY tag.

And thats it! o far it seems to work in Firefox, IE and Safari pretty well. Perhaps it will help you!
