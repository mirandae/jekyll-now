---
layout: post
title: RoboElectric 3.1.4 and Exoplayer2's SimpleExoPlayer
---

I ran into a minor bug (feature?) when writing unit tests about SimpleExoPlayer's behavior with RoboElectric. If you're unfamiliar with them, RoboElectric allows you run Android unit tests on your own machine
(no Android device required!). ExoPlayer2 is the new release of Google's ExoPlayer media player. There isn't currently a ton of documentation out about the new release, so I figured I'd write up a few points.

SimpleExoPlayer is a basic media player class that's part of the Exoplayer2 library. It has a few limitations, but it's simple and useful for most Android developers. 

If you construct an instance of SimpleExoPlayer, you get a couple of function calls dereferencing Context.CAPTIONING_SERVICE.
 No problem, except RoboElectric 3.1.4 and previous don't emulate that service. As a result, you get a couple of NPEs that are pretty easy to get rid of. 

If you're not using Mockito, you can use PowerMock mock out the requisite objects that are causing the exceptions. 

If you're using Mockito, PowerMock has some incompatibilities with it so that's not an option (As of 12/16 -- Apparently those problems will be fixed in the first couple of weeks in January, so this post will hopefully only be relevant for the next ~2 weeks).

Instead, you can first mock out the CaptioningManager.

But this doesn't get rid of all of the exceptions. You're also going to use *reflection*. Reflection is a Java feature that basically allows the currently running code to examine itself. Functions like ClassOf or Class.isInstance are examples of using reflection. It's in general not considered to be good style, but I couldn't think of a better (quick) way to handle this problem.

Here, we're going to use reflection to change the -- private constructor to be public. Then, we're going to just create the -- and sub it in.

And you're done! Simple enough, but I thought it was a nice way to see reflection.