---
title: Android MIDI
date: 2023-04-24 16:47:00
tags:
---

Back in the mid 1980s Atari STs met our MIDI needs by having built in MIDI ports. Later on, PC soundcards included MIDI adaptor sockets and now PCs and Macs support MIDI via USB. Better, there's now loads of MIDI software for desktop including DAWs and even web browsers support [Web MIDI](https://developer.mozilla.org/en-US/docs/Web/API/Web_MIDI_API). 

But what about Android phones (I have no interest or knowledge of iPhones)? They are easily more powerful that those original Atari boxes. And much more portable, if you can live with a touch interface on a small screen. Though of course there are also larger tablets and keyboards.

It turns out MIDI can work on Android though there are two main issues:

- Apps that support midi
- Connectivity

I thought I'd try a really simple MIDI setup to support my ear training using a MIDI keyboard playing sounds on the phone. I need to sing along to audio files from the Improvise for Real (IFR) website (though they can also be downloaded, I have a random selection web app) The setup is: 

- Samsung Galaxy S22+
- AKAI MPK mini keyboard with USB-C to A adapter
- [Mini Piano Pro app](https://play.google.com/store/apps/details?id=umito.android.minipiano_pro&pli=1)
- Audio tracks played with Firefox, my default browser

The piano app supports MIDI in, listening on all channels by default. It plays the sounds and highlights the key on a virtual piano. That just works. There's no support for drum sounds for the pads even though the AKAI sends them on channel 10 as usual.

A couple of issues here:

- Phone is acting as USB host and powering the AKAI USB device, but is not powered itself
- Phone sound is rubbish
- No audio mixer for app levels, or volume controls on piano app or Firefox

I've yet to sort out a mixer but the sounds levels are just OK, fortunately.

For the power and sound, chucking some hardware at the problem should work: 

- Headphones or powered speaker with wired aux for decent sound with low latency
- Anker 8195 USB-C to 3.5mm stereo adapter 
- [Anker 8346 7 in 1 hub](https://www.anker.com/uk/products/a8346) with pass-through power

![Android MIDI setup](/images/android-midi.jpg)
<figcaption>MIDI keyboard, headphones and hub (phone missing)</figcaption>

This works, mostly.

In practice this suffers from intermittent lack of sound until replugging leads brings it back.

The root cause appears to be the fact that like many phone manufacturers Samsung have ditched the 3.5mm analog audio jack, forcing us to use the single USB-C connector for analog audio, power and digital MIDI in. 

I guess this is largely due to [USB-C audio](https://www.soundguys.com/usb-audio-explained-18563/) being a mess with many options including analog and digital audio and different versions of the specs. When you buy phones or USB-C to audio adapters you don't know what options are available or being used and have no control over what's going on. With a hub in the middle working sound is hit and miss. Plus either the powered hub or the phone can be the USB host, adding another level of complexity. I expect the audio is digital to the 3.5mm adapter in this config.

I found unplugging and replugging leads from the hub or phone is required more often than not to get any sound out. For example plug in the audio adapter and phone to the hub and you get sound, until you plug in the power adapter to the hub. It seems pretty indeterminate to be honest. At least I've not found a sequence of connections that always works. It's possible the phone is the problem by not expecting power in and audio out the same connector. Or perhaps having the hub in the middle breaks whatever detection is used for audio output. 

I even updated my cheap-o USB-C audio adapter to an Anker one but it only improved things a little, maybe not at all given the intermittent nature of the issue.

This is manageable, if annoying. At some time I'll try to dig into the USB-C spec and find tools to give some visibility on just what's going on. For now I just swear a lot!

And this is only MIDI in. Having full bidirectional MIDI is probably going to be fun. I have ideas for a simple Web app to record MIDI.