---
comments: true
date: 2007-12-03 09:04:24
layout: post
slug: the-obligatory-snippets-post
title: The Obligatory Snippets Post
wordpress_id: 45
categories:
- actionscript-3
- snippets
---

There are a bunch of code snippets that I always forget.  In an attempt to organize my life, I'm going to throw them up here.  Hopefully Matt will too.




### Removing all Children From a DisplayList



    
    
    while( numChildren > 0 )
    {
    	removeChildAt( 0 );
    }





### Make Sure Everything is on the Whole Pixel


This is just a useful concept to throw out there, since everything should be on the whole pixel if you'd like it to look crisp.

    
    
    override public function set x( value:Number ):void 
    {
    	super.x = Math.round( value );
    }


Pretty easy huh? Just do the same for width/height and y.
