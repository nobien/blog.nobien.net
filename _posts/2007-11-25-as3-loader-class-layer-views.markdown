---
comments: true
date: 2007-11-25 07:22:03
layout: post
slug: as3-loader-class-layer-views
title: 'AS3: Loader Class & layer views.'
wordpress_id: 44
categories:
- actionscript 3
---

I ran into this weird problem while working on something for Almighty.  I've done a small amount of research on the topic and found nothing.  I apologize if the answer is blatantly obvious ... but I'm stumped.

I have two MovieClips that load in dynamic content using the Loader class:

    
    package
    {
        import flash.net.URLRequest;
        import flash.display.Loader;
        import flash.display.MovieClip;
    
        class ImageHolder extends MovieClip
        {
            public function ImageHolder( urlReq:URLRequest )
            {
                var loader:Loader = new Loader();
                loader.load( urlReq );
                addChild( loader );
            }
        }
    }


Pretty simple huh?  Normally I'd add all of the fun loving event listeners, but not today.
Now I load some external data in... a SWF containing a PNG, and a JPG.

    
    package
    {
        import flash.net.URLRequest;
        import flash.display.MovieClip;
    
        class MyMovieDocument extends MovieClip
        {
            public function MyMovieDocument()
            {
                var imageHolder1:ImageHolder = 
                    new ImageHolder( new URLRequest( "/images/some.jpg" ));
                
                var imageHolder2:ImageHolder = 
                    new ImageHolder( new URLRequest( "/swfs/some.swf" ));
    
                addChild( imageHolder1 );
                addChild( imageHolder2 );
            }
        }
    }


Easy enough.  Well I'm baffled by the results.  When you load both images in and add them to the display list, the JPG is on top even though it was added first.  Heck, I've even tried using addChildAt( imageHolder1 , 0 ); and still the same result... The JPG is always on top.  This is weird since they're wrapped in individual MovieClips.

I have the result swf but I can't get it to show up in WordPress, and I don't have the time right now to figure it out.

Either way, this is a weird issue.  Has anyone else had this problem?  Or am I just missing something?
