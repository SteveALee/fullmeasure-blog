---
title: Audacity with ASIO
date: 2020-03-28 16:47:00
tags:
---

Whenever I want to do a quick recording, possibly with a few tweaks, my goto programme is the venerable open source success story [Audacity](https://www.audacityteam.org/). This multi track audio recorder with processing effects is powerful enough to do much audio editing. It's simple enough to get your head round and so ideal for a quick recording when you don't need a full DAW (eg Reaper).

When using Audacity with [k-mix](../27/k-mix-and-windows) I found there was a bit of a driver issue. The current ASIO k-mix driver only works with Audacity's MME mode and is restricted to 2 channels. I wanted to record channels 3 & 4 so looked into building Audacity with ASIO support as the option is only available for self build and not redistribution for licensing reasons.

![Audacity showing 8 channels using the ASIO driver](/images/audacity-ASIO.jpg)

Eventually I discovered that the ASIO build was actually not much use for my requirements as I could use the k-mix flexibilioty to send channnels 3 & 4 to 1 &2 and use the release Audacity. But, in case others are interested in using it, I provide a simple [Windows cmd script](https://gist.github.com/SteveALee) to help automate the build.

It's worth emphasising that the ASIO build does not provide any improvement in recording quality over using the MME option. ASIO drivers do provide lower latency but that is unlikely to be relevant. More useful, is the access to all 8 k-mix channels.

Whilst having access to all 8 channels might seem useful, Audacity's support for more than 2 channels is poor. To record channels 3 & 4, say, you have to record 1, 2, 3 & 4 and the level meters are fixed to channels 1 & 2. Other features like pan control for Left or right make little sense.
