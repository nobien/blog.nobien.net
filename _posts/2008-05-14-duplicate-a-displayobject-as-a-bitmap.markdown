---
comments: true
date: 2008-05-14 03:35:12
layout: post
slug: duplicate-a-displayobject-as-a-bitmap
title: Duplicate a DisplayObject as a BitMap
wordpress_id: 63
categories:
- actionscript-3
- flash
- flex
- snippets
---

In working on a project, I created a simple utility class that has this public static method. This basically takes a DisplayObject, copies it, and adds it to a new Sprite which is then returned to the caller. Simple enough. I'm sure this can be refined or done a different way, so have at it.


    
    public static function duplicateImageAsSprite( original:DisplayObject ):Sprite 
    {
        var bitmapData:BitmapData = new BitmapData( original.width , original.height , 
            true , 0x000000 );
        bitmapData.draw( original as IBitmapDrawable );
    			
        var bitmap:Bitmap = new Bitmap( bitmapData );
    
        var returnSprite:Sprite = new Sprite();
        returnSprite.addChild( bitmap as DisplayObject );
    			
        return returnSprite;
    }
    
