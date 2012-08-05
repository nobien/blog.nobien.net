---
comments: true
date: 2007-10-31 08:57:17
layout: post
slug: as3bloggerlib-v013-update
title: AS3BloggerLib v0.1.3 Update
wordpress_id: 40
categories:
- actionscript-3
- air
- flex
---

Today I updated the AS3BLoggerLib project. Recently I realized that I wasn't handling service responses in a good way. Previously, I was assuming that when the URLLoader's COMPLETE event was dispatched that the service call was successful. This obviously is not the case. Now a simple check on the HTTP status code ensures that a BloggerService event is dispatched only when a successful call has been made. Otherwise, an ErrorEvent is dispatched with the message from the server.

I still have yet to receive feedback from anyone concerning this library. I know very few people have downloaded it, but if you have, and if you've attempted using it all, I'd love to hear from you.
