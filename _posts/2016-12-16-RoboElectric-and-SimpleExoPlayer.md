---
layout: post
title: RoboElectric 3.1.4 and Exoplayer2's SimpleExoPlayer
---

I ran into a minor bug (feature?) when writing unit tests about SimpleExoPlayer's behavior with RoboElectric. If you're unfamiliar with them, RoboElectric allows you run Android unit tests on your own machine
(no android device required!). ExoPlayer2 is the new release of Google's ExoPlayer media player. There isn't currently a ton of documentation out about the new release, so I figured I'd write up a few points.

SimpleExoPlayer is a basic media player class that's part of the Exoplayer2 library. It has a few limitations, but it's simple and useful for most Android developers. 

If you construct an instance of SimpleExoPlayer, you get a couple of function calls dereferencing Context.CAPTIONING_SERVICE.
 No problem, except RoboElectric 3.1.4 and previous don't emulate that service. As a result, you get a couple of NPEs that are pretty easy to get rid of. 


