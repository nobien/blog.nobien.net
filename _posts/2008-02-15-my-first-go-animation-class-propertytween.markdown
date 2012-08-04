---
comments: true
date: 2008-02-15 22:49:42
layout: post
slug: my-first-go-animation-class-propertytween
title: 'My First Go Animation Class: PropertyTween'
wordpress_id: 50
categories:
- actionscript 3
---

So I finally decided to have a go (no pun intended) with Moses new [Go tween framework](http://www.goasap.org). I've only scratched the surface, but its pretty cool how easy it was to create a simple tween class that can tween the properties of any object.



    
    /**
    * Simple Property Tween CLass
    * @author Matt Wright
    * @version 0.1
    */
    
    package net.nobien.tween
    {
    
        import org.goasap.items.LinearGo
    
        public class PropertyTween extends LinearGo
        {
            private var _target:Object;
            private var _props:Array;
            private var _endValues:Array;
            private var _startValues:Array;
            private var _changeValues:Array;
    
            public function PropertyTween( target:Object,
                                           props:Array,
                                           values:Array,
                                           delay:Number = 0,
                                           duration:Number = 1,
                                           easing:Function = null,
                                           autoStart:Boolean = true )
            {
                super( delay, duration, easing );
                _target = target;
                _props = props;
                _endValues = values;
                if( autoStart ) start();
            }
    
            override public function start():Boolean
            {
                _startValues = new Array();
                _changeValues = new Array();
    
                for ( var i:int = 0; i < _props.length; i++ )
                {
                    _startValues.push( _target[_props[i]] );
    
                    var d:Number = ( useRelative ) ? _endValues[i]
                                                   : _endValues[i] - _target[_props[i]];
                    _changeValues.push( d );
                }
    
                return super.start();
            }
    
            override protected function onUpdate( type:String ):void
            {
                for ( var i:int = 0; i < _props.length; i++ )
                {
                    _target[_props[i]] = correctValue( _startValues[i] +
                        _changeValues[i] * _position );
                }
            }
    
        }
    
    }


Its pretty easy to use here's some quick test code:

    
    package
    {
    
        import com.robertpenner.easing.Expo;
        import flash.display.Sprite;
        import net.nobien.tween.PropertyTween;
    
        public class TweenTest
        {
    
            public var propTween:PropertyTween;
            public var clip:Sprite;
    
            public function TweenTest()
            {
                clip = new Sprite();
                clip.graphics.beginFill( 0x000000 );
                clip.graphics.drawRect( 0, 0, 20, 20 );
                clip.x = 50;
                clip.y = 50;
                addChild( clip );
    
                propTween = new PropertyTween( clip,
                                               ["x", "y", "alpha"],
                                               [400, 200, 0.2],
                                               1, 3, Expo.easeOut );
            }
    
        }
    
    }
