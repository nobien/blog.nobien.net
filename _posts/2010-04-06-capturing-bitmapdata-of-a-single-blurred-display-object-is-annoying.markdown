---
comments: true
date: 2010-04-06 16:48:41
layout: post
slug: capturing-bitmapdata-of-a-single-blurred-display-object-is-annoying
title: Capturing BitmapData of a Single Blurred Display Object is Annoying
wordpress_id: 219
categories:
- actionscript 3
- flash
---

I feel like I'm going crazy here. I have a circle inside a MovieClip. The (x,y) position of the circle inside the MovieClip is (0,0).  I'm applying a BlurFilter to the MovieClip the circle is in. I'd like to create some BitmapData based on the filtered MovieClip. Here's my code.

    
    var blur:BlurFilter = new BlurFilter( 20, 20 );
    
    circle2.filters = [blur];
    
    var bd:BitmapData = new BitmapData( circle2.width + blur.blurX,
        circle2.height + blur.blurY, true, 0x00FFFFFF );
    var clipRect:Rectangle = bd.generateFilterRect(
        circle2.getRect( null ), blur );
    trace( clipRect ); // (x=-10, y=-10, w=70, h=70)
    bd.draw( circle2, null, null, null, clipRect );
    
    var bm:Bitmap = new Bitmap( bd );
    bm.x = 385;
    bm.y = 31;
    addChild( bm );


And here's the result:

[kml_flashembed publishmethod="static" fversion="9.0.0" movie="http://blog.nobien.net/wp-content/uploads/2010/04/filterRectTest.swf" width="500" height="150" targetclass="flashmovie"]

[![Get Adobe Flash player](http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif)](http://adobe.com/go/getflashplayer)

[/kml_flashembed]

I'd like to think that using the clipRect that's returned from the bd.generateFilterRect() method in the bd.draw() method would properly define the capture area of the MovieClip, but it always gets cropped wrong. Am I crazy? I really feel like this should work. I've fiddle around with the values and I still can't seem to get it to work.

The only work around I was able to come up with was to put the circle MovieClip in a container and adjust the X and Y position of the container based on the blurX and blurY values, but that just seems annoying to have to do!

Any suggestions?

**Update!**
Thanks to Dan (see comment below) to letting me know whats up. I was a total moron. I tend to shy away from Matrix objects because I'm scared of them (only becase I don't know how to use them). Works like a charm now. Check it:

[kml_flashembed publishmethod="static" fversion="9.0.0" movie="http://blog.nobien.net/wp-content/uploads/2010/04/filterRectTest2.swf" width="500" height="150" targetclass="flashmovie"]

[![Get Adobe Flash player](http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif)](http://adobe.com/go/getflashplayer)

[/kml_flashembed] 
