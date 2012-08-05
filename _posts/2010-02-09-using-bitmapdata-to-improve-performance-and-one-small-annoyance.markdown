---
comments: true
date: 2010-02-09 17:37:49
layout: post
slug: using-bitmapdata-to-improve-performance-and-one-small-annoyance
title: Using BitmapData to Improve Performance (And One Small Annoyance)
wordpress_id: 199
categories:
- actionscript-3
- flash
---

Lately I've been experimenting with generative art in Flash/ActionScript 3. Its been pretty fun and I've created some interesting results. In my experience, and I can only assume in most other artists and developers cases, I really got familiar with the Flash Player's performance limitiations. More specifically, I came to truly find out just how bad the Flash Player is at rendering a lot of display objects at once, even if they're just Shape objects, even if cacheAsBitmap = true, and even if they're not animating in any way. This, of course, forces you to look into performance enhancing techniques.



The most common technique that I see artists and developers using involves  rendering bitmaps from a display object. Rendering a bitmap of your display objects is a great way to keep the memory of your piece under control. If you were to just leave every Shape, Sprite or MovieClip you created on the stage, the memory your piece consumes would continually grow and eventually cripple your piece. So what you do is render a bitmap every so often and discard any "dead" display objects. By "dead" I mean display objects that are not animating or updating themselves in any way. Its a pretty simple technique. The following example illustrates how this is done:

[kml_flashembed publishmethod="dynamic" fversion="10.0.0" replaceId="bitmaptest01" movie="/examples/bitmapdata/BitmapTest01.swf" width="500" height="300" targetclass="flashmovie"]

[![Get Adobe Flash player](http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif)](http://adobe.com/go/getflashplayer)

[/kml_flashembed]

The code for this example looks like so:

    
    import flash.display.Sprite;
    import flash.display.Shape;
    import flash.display.Bitmap;
    import flash.display.BitmapData;
    import flash.events.MouseEvent;
    
    var circles:Sprite = new Sprite();
    
    var bitmapData:BitmapData = new BitmapData( stage.stageWidth,
    	stage.stageHeight, true, 0x00FFFFFF );
    bitmapData.draw( circles );
    
    var bitmap:Bitmap = new Bitmap( bitmapData );
    addChild( bitmap ); 
    
    function onStageClick( event:MouseEvent ):void
    {
    	for( var i:int = 0; i < 10; i++ )
    		circles.addChild( getRandomCircle() );
    	bitmapData.draw( circles );
    	while( circles.numChildren )
    		circles.removeChildAt( 0 );
    }
    
    function getRandomCircle():Shape
    {
    	var c:Shape = new Shape();
    	c.graphics.beginFill( Math.random() * 0xFFFFFF );
    	c.graphics.drawCircle( 0, 0, Math.random() * 100 );
    	c.graphics.endFill();
    	c.x = Math.random() * stage.stageWidth;
    	c.y = Math.random() * stage.stageHeight;
    	return c;
    }
    
    stage.addEventListener( MouseEvent.CLICK, onStageClick );


In this example a bitmap is rendered based on the contents of the circles Sprite object every time the user clicks the stage. However, before the bitmap is rendered, 10 random circles are placed inside the circles Sprite. After the bitmap is rendered, the random circles are removed. One of the best parts of rendering a bitmap is that you don't have to continually create new BitmapData or Bitmap objects when its time to render. If you add more display objects to your subject and want to render them in the bitmap you just have to call the draw() method on the BitmapData object again. Also, notice that the memory used by the piece doesn't increase over time even thought it looks like you're adding hundreds of display objects to the stage. Cool stuff.

**The One Small Annoyance**

As I've been messing around with this stuff I came across a little oddity which caused me great annoyance for a few hours. I decided to start adding filters to the shapes I was creating. No big deal, right? Well, lets take a look at what I did by adjusting the previous example to apply a BlurFilter instance on each circle that's created.

[kml_flashembed publishmethod="dynamic" fversion="10.0.0" replaceId="bitmaptest02" movie="/examples/bitmapdata/BitmapTest02.swf" width="500" height="300" targetclass="flashmovie"]

[![Get Adobe Flash player](http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif)](http://adobe.com/go/getflashplayer)

[/kml_flashembed]

The code for this example looks like so:

    
    import flash.display.Sprite;
    import flash.display.Shape;
    import flash.display.Bitmap;
    import flash.display.BitmapData;
    import flash.events.MouseEvent;
    import flash.filters.BlurFilter;
    
    var circles:Sprite = new Sprite();
    
    var bitmapData:BitmapData = new BitmapData( stage.stageWidth,
    	stage.stageHeight, true, 0x00FFFFFF );
    bitmapData.draw( circles );
    
    var bitmap:Bitmap = new Bitmap( bitmapData );
    addChild( bitmap ); 
    
    function onStageClick( event:MouseEvent ):void
    {
    	for( var i:int = 0; i < 10; i++ )
    		circles.addChild( getRandomCircle() );
    	bitmapData.draw( circles );
    	while( circles.numChildren )
    		circles.removeChildAt( 0 );
    }
    
    function getRandomCircle():Shape
    {
    	var c:Shape = new Shape();
    	c.graphics.beginFill( Math.random() * 0xFFFFFF );
    	c.graphics.drawCircle( 0, 0, Math.random() * 100 );
    	c.graphics.endFill();
    	c.x = Math.random() * stage.stageWidth;
    	c.y = Math.random() * stage.stageHeight;
    	c.filters = [new BlurFilter( 150, 150 )];
    	return c;
    }
    
    stage.addEventListener( MouseEvent.CLICK, onStageClick );


You should notice occasionally that when the bitmap is rendered there are some clipping issues. You might see some hard lines on a blurred circle as if it was masked. This not only occured for BlurFilter instances, but also any other filters with blur-like effects, such as DropShadowFilter and GlowFilter. The following screen shot illustrates this:

![Clipping Screenshot](http://blog.nobien.net/examples/bitmapdata/clipping.jpg)

I couldn't for the life of me figure out what was causing this to occur until I accidentally stumbled across a fix while creating these examples. **To avoid the clipping, simply add the display object that you're rendering to the stage**. Why this works is beyond me, but at least its a fix. As an additional performance measure, you can set the visibility of that display object to false. In the case of these examples we're talking about the circles Sprite. Adjust the line where the circles Sprite is created to look like this:

    
    var circles:Sprite = new Sprite();
    addChild( circles );
    circles.visible = false;


And the resulting piece should render properly like so:

[kml_flashembed publishmethod="dynamic" fversion="10.0.0" replaceId="bitmaptest02" movie="/examples/bitmapdata/BitmapTest03.swf" width="500" height="300" targetclass="flashmovie"]

[![Get Adobe Flash player](http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif)](http://adobe.com/go/getflashplayer)

[/kml_flashembed]

While this is just a basic example, this technique can be applied in many different ways to improve performance. For example, I've been working on a painting program that paints a loaded image using organic moving brushes that uses a similar technique. The brushes even animate a bit before the are discarded. At any rate, I hope to find more time to be able to share more things that I find useful while working with generative art. Until then, please post questions or any suggestions to techniques you find useful!
