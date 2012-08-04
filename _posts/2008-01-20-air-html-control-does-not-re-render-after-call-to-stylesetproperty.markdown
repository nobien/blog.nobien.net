---
comments: true
date: 2008-01-20 16:20:04
layout: post
slug: air-html-control-does-not-re-render-after-call-to-stylesetproperty
title: AIR HTML control does not re-render after call to style.setProperty()?
wordpress_id: 46
categories:
- actionscript 3
- air
- flex
---

So I'm working on a small application that allows you to edit the HTML and CSS of a page that is loaded into an AIR HTML control. I can easily reset the the value of the body's innerHTML property and the HTML control re-renders the view. However, on the CSS side of things, it doesn't seem to re-render. Here's an example of the code from my application:

    
    var win:Object = html.htmlLoader.window;
    win.document.styleSheets[i].cssRules[n].style.setProperty( prop, value );


Simple enough, no? Well whats annoying is that this used to work in Beta 2 so long as I set the body's innerHTML property equal to itself as sort of a refresh call. I've even traced the value of the style's property before and after the call to setProperty() to double check that it had been changed, and of course it was fine.

So my question is if anyone has experienced this or if they know that this is as designed? It was sorta crucial to the design of my application from beta 2 and I'd really like to get it working again.
