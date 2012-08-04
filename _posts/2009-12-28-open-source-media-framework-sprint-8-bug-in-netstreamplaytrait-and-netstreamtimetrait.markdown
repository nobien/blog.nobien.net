---
comments: true
date: 2009-12-28 17:01:45
layout: post
slug: open-source-media-framework-sprint-8-bug-in-netstreamplaytrait-and-netstreamtimetrait
title: Open Source Media Framework (Sprint 8) Bug in NetStreamPlayTrait and NetStreamTimeTrait?
wordpress_id: 184
categories:
- actionscript 3
- air
- flash
- flex
- frameworks
---

Over the last two months or so I've been keeping my eyes on Adobe's [Open Source Media Framework](http://www.opensourcemediaframework.com/) (OSMF) project. I started fiddling around with Sprint 7, seeing if I could build a progressive video player component on top of it. My first impression was that the project was still in its infancy solely based on the naming conventions of the framework's events. It just didn't feel intuitive. Additionally, there seemed to be some bugs surrounding the seeking of progressive videos. Alas, the development team has made some major improvements in Sprint 8 with regards to both of these items.

Upon downloading Sprint 8 I was immediately happy with the renaming of the framework event classes. They make much more sense now. Seeking progressive videos seemed to have improved as well, but I was still experiencing some buggy behavior when continuously scrubbing a progressive video.



My conclusion is that there is a bug in the NetStreamPlayTrait class and NetStreamTimeTrait classes. Basically, the NetStream.Play.Stop status code is sometimes dispatched when it shouldn't be. For instance, when I continuously scrub a video using the seek() method, there are times at which the aforementioned status code is dispatched when the playhead isn't at the very end of the video. More specifically, when I scrub to the end of the video for the first time the status code is dispatched and caught by the mentioned framework classes. Thus, this causes two things to happen.

First, the NetStreamTimeTrait class dispatches a TimeEvent.DURATION_REACHED event whenever this status code is received. Thus, the MediaPlayer redispatches this event at times when the playhead is not at the end, causing registered listeners to think the playback has completed when it hasn't. This was very strange, so I put some trace statements in the onNetStatus event handler of this class to check the currentTime and duration properties. When this event was dispatched during continuous scrubbing these values were never the same and often not close to being equal at all. Even when the video reached the duration during playback, these values weren't always equal. In one case the currentTime property was larger than the duration by one decimal place. I'm guessing this is a discrepancy between the NetStream.time property and the value of the duration provided by the meta data. At any rate, I thought it might be good to add a condition into the status handler to check if the currentTime was equal to the duration, thus verifying the playhead has in fact truly reached the end of the video. First, I edited the currentTime setter to look like this:

    
    override public function get currentTime():Number
    {
        return Math.min( duration, netStream.time );
    }


This ensures that the currentTime is never larger than the duration and avoids the bug I described earlier. Next I added a condition into the onNetStatus handler of the NetStream.Play.Stop code. It now looks like this:

    
    private function onNetStatus(event:NetStatusEvent):void
    {
        switch (event.info.code)
        {
            case NetStreamCodes.NETSTREAM_PLAY_STOP:
                // For progressive,	NetStream.Play.Stop means the duration
                // was reached.  But this isn't fired for streaming.
                if (NetStreamUtils.isRTMPResource(resource) == false &&
                    currentTime == duration)
                {
                    processDurationReached();
                }
                break;
        }
    }


Notice the additional condition to check if the currentTime is equal to the duration. This ensures that the TimeEvent.DURATION_REACHED event isn't dispatched until the playhead time has reached the duration. These two fixes took care of the TimeEvent.DURATION_REACHED event from being dispatched when it shouldn't be, however I was still experiencing a problem when I tried to resume playback of the progressive video if I happened to scrub it to the end.

Just as in the NetStreamTimeTrait class, the NetStreamPlayTrait class handles the NetStream.Play.Stop status code. As mentioned, the status code tends to get dispatched when it shouldn't, especially when continuously scrubbing and you happen to scrub to the end of the video. So, in the NetStreamPlayTrait class is that when this status code is dispatched, the following code is executed:

    
    case NetStreamCodes.NETSTREAM_PLAY_STOP:
        // Fired when streaming connections buffer, but also when
        // progressive connections finish.  In the latter case, we
        // halt playback.
        if (urlResource != null && 
            NetStreamUtils.isRTMPStream(urlResource.url) == false)
        {
            // Explicitly stop to prevent the stream from restarting on seek();
            streamStarted = false;
            stop();
        }
        break;


So for progressive videos, the streamStarted property is set to false and the video is stopped. Confused, I followed the stop method around and determined that is just pauses the NetStream object. Then I looked around for where the streamStarted property is used and found it being used within the processPlayStateChange() method. From what I can tell it's used to denote if stream/video data exists in the NetStream object. Why would this be set to false for a progressive video's NetStream.Play.Stop status? Is not the video data still in memory? So I commented out that line and the code now looks like this:

    
    case NetStreamCodes.NETSTREAM_PLAY_STOP:
        // Fired when streaming connections buffer, but also when
        // progressive connections finish.  In the latter case, we
        // halt playback.
        if (urlResource != null && 
            NetStreamUtils.isRTMPStream(urlResource.url) == false)
        {
            // Explicitly stop to prevent the stream from restarting on seek();
            //streamStarted = false;
            stop();
        }
        break;


Upon testing my video player my continuous scrubbing seems to behave properly! Woohoo! Now let me just say that I have not tested this extensively and I know that OSMF is still in development. I could very well have created a new bug by doing this, but at least things are behaving the way I would assume they would now.
