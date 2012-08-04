---
comments: true
date: 2008-03-28 09:38:13
layout: post
slug: as3-bufferingindicator-class
title: AS3 BufferingIndicator Class
wordpress_id: 51
categories:
- actionscript 3
- flash
- flex
---

Out of boredom the other day I wrote this class to create a simple animated buffering indicator, you know...the ones we always see everywhere that look like a pinwheel.  Here's an example of what you can do with it:



[kml_flashembed movie="/wp-content/uploads/2008/03/bufferingindicator.swf" height="180" width="380" /]

Here's the code that generated this example:

    
    var bi:BufferingIndicator = new BufferingIndicator();
    bi.x = 40
    bi.y = 45
    this.addChild( bi );
    
    bi = new BufferingIndicator( 50, 20, 7, 20, 0xCC0000, 6, 0.2, 25 );
    bi.x = 130;
    bi.y = 65;
    this.addChild( bi );
    
    bi = new BufferingIndicator( 75, 40, 8, 25, 0x0000FF, 10, 0.2, 100 );
    bi.x = 270;
    bi.y = 90;
    this.addChild( bi );


And here's where you can download the class file: [BufferingIndicator.as](http://blog.nobien.net/wp-content/uploads/2008/03/bufferingindicator.as)
