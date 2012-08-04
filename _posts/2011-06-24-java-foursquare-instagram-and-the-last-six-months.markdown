---
comments: true
date: 2011-06-24 15:32:25
layout: post
slug: java-foursquare-instagram-and-the-last-six-months
title: Java, Foursquare, Instagram, and the Last Six Months
wordpress_id: 362
categories:
- general
- Java
- Social Media
- Spring Framework
tag:
- foursquare
- instagram
- java
- spring framework
- spring social
---

After a bit of a hiatus, it's time to start writing again. In the last six months I've been familiarizing myself with as many different technologies and tools as possible. There has been, though, a focus on Java, [Spring Framework](http://www.springsource.org/about), and it's various sub projects such as [Spring Social](http://www.springsource.org/spring-social).

I gave Spring Social a try a a few months ago when building a little prototype for a project at work. It made integrating with sites like Facebook and Twitter so ridiculously easy. For whatever reason I was expecting a lot of headaches so I was pleasantly surprised. So much so that I was motivated to write an [API binding for Instagram](https://github.com/mattupstate/spring-social-instagram). It was a good way to cut my teeth as their API isn't too complicated. Nearly all API endpoints are covered aside from some sort of implementation against their real-time API. Also, it's not officially released by SpringSource so you won't find it in any of their repositories or the central Maven repo. Check out the tiny [example project](https://github.com/mattupstate/spring-social-instagram-example) to get started with it. Or better yet, fork it and contribute.

To my surprise, [@poornerd](http://twitter.com/#!/poornerd) was actually using it in a project of his. Check out the hist [test/beta](http://beta.locoflow.com/venue.jsp?vid=4adcda7af964a5201a4721e3&tag=smcberlin). His project also intends to use Foursquare so I decided to have a go at writing another [API binding for Foursquare](https://github.com/mattupstate/spring-social-foursquare). The Foursquare API is much more complicated than Instagram, and there are some oddities with how their objects are serialized to JSON. Thus, some of the object names are a little odd but I'm hoping to get some feedback from the community on that.  Aside from that, all major endpoints are implemented aside from the "multi" API which lets you  make more than one call with one HTTP request. I also setup an [example project](https://github.com/mattupstate/spring-social-foursquare-example) for Foursquare as well.

It's been fun contributing to an open source community and hopefully at some point they'll be officially released through SpringSource. Some feedback and/or criticisms would be greatly appreciated about either project so please go check them out if you have a moment and leave your comments here.
