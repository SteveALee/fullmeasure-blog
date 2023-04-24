---
title: Android MIDI
date: 2023-04-24 16:47:00
tags:
---

Back in the mid 1980s Atari STs met our MIDI needs. Later PC soundcards included MIDI adaptor sockets and now PCs and Macs all support USB MIDI. There's loads of MIDI software for desktop including DAWs and even web browsers support Web MIDI. 

But what about Android phones (I have no interest or knowledge of iPhones)? They are easily more powerful that those old Atari boxes. And much more portable, if you can live a touch interface on a small screen (of course there are also larger tablets).

It turns out there are two main issues:

- Apps that support midi
- Connectivity

I thought I'd try a really simple MIDI setup to support my ear training with Improvise for Real (IFR). Extrnal MIDI keyboard playing sounds on the phone.

- Samsung Galaxy S22+
- AKAI MPK mini keyboard + USB-C to A adapter
- [Mini Piano Pro app](https://play.google.com/store/apps/details?id=umito.android.minipiano_pro&pli=1)
- IFR audio tracks played with Firefox, my default browser

The piano app supports MIDI, listening on all channels by default. So that just works. No drums sounds for the pads though even though the AKAI sends them on channel 10.

A couple of issues here:

- Phone is acting as USB host and powering the AKAI, but is not powered itself
- Phone sound is rubbish
- No audio mixer or volume controls on piano app or Firefox

I've yet to sort out a mixer but the sounds levels are just OK fortunately.

For the power and sound, chucking some hardware at the problem should work: 

- Headphones or powered speaker with wired aux for decent sound with low latency
- USB-C to 3.5mm stereo adapter 
- Anker 7-way media hub with pass-through power

![Android MIDI setup](/images/android-midi.jpg)
<figcaption>MIDI keyboard, phones and hub</figcaption>

This works, mostly.

In practice this suffers from intermittent lack of sound until replugging leads brings it back.

The root cause appears to be the fact that like many phone manufacturers Samsung have ditched the 3.5mm analog audio jack, forcing us to use the single USB-C connector for analog audio, power and digital MIDI in. 

I guess this is partly as USB-C audio seems to be a mess with many options including analog and digital audio and different versions of the specs. When you buy phones or USB-C to audio adapters you don't know what options are available or being used and have no control over what's going on. With a hub in the middle working sound is hit and miss. Plus either the powered hub or the phone can be the USB host, adding another level of complexity.

I found unplugging and replugging leads from the hub or phone is required more often than not to get any sound out. For example plug in the audio adapter and phone to the hub and you get sound, until you plug in the power adapter to the hub. It seems pretty indeterminate to be honest. At least I've not found a sequence on connections that always works. It's possible the phone is the problem, not expecting power in and audio out the same connector. Or perhaps having the hub in the middle breaks what every detection is used for audio output. 

I updated by cheap-o USB-C audio adapter to an Anker one but it only improves things a little.

This is manageable, if really annoying. At some time I'll try to dig into the USB-C spec and find tools to give some visibility on just what's going on. For now I just swear a lot!

And this is only MIDI in so far. Having full bidirectional MIDI is probably going to be fun. I have ideas for a simple Web app to record MIDI.