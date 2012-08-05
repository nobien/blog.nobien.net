---
comments: true
date: 2008-11-18 21:50:56
layout: post
slug: why-no-love-for-the-monostate-pattern-in-actionscript-3
title: Why No Love for the Monostate Pattern in ActionScript 3?
wordpress_id: 96
categories:
- actionscript-3
- design patterns
---

A coworker of mine recently introduced me to the monostate pattern. At first I was like "wtf is that?". I had never heard of that pattern before. He is of the opinion that the monostate pattern is far better than the singleton pattern. I had no opinion on the subject because I only assumed singletons are the one and only way to ensure only one instance of an object is created throughout an application.



At first I gasped in excitement at the thought that there was something possibly, or arguably, better than using singletons. Like I said, I had no real opinion prior, so I spent today researching it. Low and behold, there's hardly any info or examples of implementations of this pattern out there. The only useful info I found was at this [blog post](http://www.bigroom.co.uk/blog/better-without-singletons). But, here is an example of what I gathered a monostate class to look like in ActionScript 3:

    
    package
    {
    	public class ApplicationConfiguration
    	{
    		protected static var _language:String = "en";
    
    		public function ApplicationConfiguration()
    		{
    
    		}
    
    		public function get language():String
    		{
    			return _language;
    		}
    
    		public function set language( v:String ):void
    		{
    			_language = v;
    		}
    	}
    }


So in this case I have a class with a protected static var named _language in it. This variable, because its static, will only ever exist in one place, and thats in the class definition. No matter how many times I create a new ApplicationConfiguration instance, the value of that variable will persist throughout each instance, allowing every instance to have a "mono state". Why didn't I ever think of this before? It's so simple and clever. So here's an example use of this class:

    
    var config1:ApplicationConfiguration = new ApplicationConfiguration();
    trace( config1.language ); // Output: en
    config1.language = "fr";
    
    var config2:ApplicationConfiguration = new ApplicationConfiguration();
    trace( config2.language ); // Output: fr
    config2.language = "es";
    
    trace( config1.language ); // Output: es


I've commented in the output of that code, and you can see, when I make a change to the language property in config2 the value of language in config1 also changes.

So now I'm wondering what the advantages and disadvantages to this approach over a singleton might be. One of the most obvious advantages I saw right away was the fact that I can easily extend a monostate class. Extending singletons is, well, impossible as far as I know. Other than that I'm not too sure what some major advantages are. But extending seems to be big enough for me to want to start using this pattern.

At any rate, I'd like to hear some comments about anyone's experience with this pattern.
