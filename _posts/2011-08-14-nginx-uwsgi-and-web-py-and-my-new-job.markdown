---
comments: true
date: 2011-08-14 21:16:55
layout: post
slug: nginx-uwsgi-and-web-py-and-my-new-job
title: Nginx, uWSGI, and web.py...and my new job
wordpress_id: 382
categories:
- general
---

This past week I started a new job at [Local Projects](http://www.localprojects.com). I'm really excited about working with all the talented people there. One of the fun things about the job so far is that I'm doing a bunch of Python development with [web.py](http://www.webpy.org). It's a great minimal web framework and working with Python has been great as well.

One of the things I wanted to do this weekend was to figure out how to run a web.py app with [nginx](http://www.nginx.org/) and [uWSGI](http://projects.unbit.it/uwsgi/wiki). It took me ages to figure it out, but I finally got a working setup. This post is mostly to document the few things that made the difference.

First thing to note is that I did this all on a micro instance of Ubuntu 11.04 on EC2 ([ami-1aad5273](https://console.aws.amazon.com/ec2/home?region=us-east-1#launchAmi=ami-1aad5273)). Second, I installed the lastest versions of nginx and uWSGI from source. The following gist lists examples of the files I used to get this running. I should note that the config and .sh files are intended for running one server and one web.py application.


