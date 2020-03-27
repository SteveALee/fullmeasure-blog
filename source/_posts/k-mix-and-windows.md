---
title: K-mix and WIndows
date: 2020-03-27 16:47:00
tags:
---

Watching [Steve Lawson playing](https://youtu.be/l2zWzpvhOVQ?t=24) playing with his setup you may notice a couple of interesting devices with white shapes on them. These are Keith McMillen [k-mix(https://www.keithmcmillen.com/products/k-mix/)] mixer, digital interface come control surface and [QuNeo](https://www.keithmcmillen.com/products/quneo/) pad controller. The white areas are McMillen's unique touch 'fabric' which provides unique touch control dynamics.

Since giving my old Spirit Folio Notepad mixer to the charity I work for that records news for the blind I have occasionally required a small mixer suitable for connecting a Rode NT-1 condenser mic and bass guitar to a WIndows PC for recording.

In addition, I'd like to get creative with digital music using a DAW and flexible digital interface. Last time I messed around live this was on an Atari.

Having chatted to Steve it's clear he holds the McMillen products in high regard and the K-mix sounded unique in its capabilities and flexibility. And at a good price too. Windows support is now available in the form of a Windows version of the editor and a ASIO driver.

![Laney KB80 combo](/images/k-mix.jpg)

My first task was to record a voice over for a video introducing a cognitive accessibility project I'm working on. This was a very simple setup: mic XLR into the k-mix input 1 with it's excellent pre amp, 48v phantom power on, monitor amp on output 1 and USB lead to PC and Audacity to record channel 1.

It didn't work quite a expected, the level was very low. But it turns out that was my not understanding the default config. I was expecting the USB channels to mirror the outputs, at least for ch 1 & 2. But that's not how the k-mix is configured and on reflection that's due to the more common use cases for the k-mix. So as a stand alone digital mixer with analogue inputs an outputs, it works beautifully as you'd expect. However as a digital interface the default settings bypass  the mixer controls, sending a pre fader feed to USB channel 1. However the channel gain (trim as it's called) is pre the feed so does have effect on the level. By adjusting that I was able to get a decent recording level and complete the job.

Of course, the k-mix lets you set the digital usb feed to be post fader. This setting, like a few others can only be changed using the K-Mix editor program. And this is where I hit a blocking problem. The editor would not run for me. However talking to Evan at McMillen, it turns out a recent update to WIndows seems to have caused the problems I was experiencing. The issues were: Editor would not run as it got stuck in an upgrade loop from from 1.3.5 to 1.3.5 and the windows was so small the text was clipped.

While McMillen work on a fix to the problems caused by the Windows update, a temporary work around is to use Windows' compatibility feature as follows:

- right click on "K-Mix Editor.exe" in "Program files".
- select "Properties"
- select the "Compatibility" tab at the top
- check "run this in compatibility mode for:"
- choose "Windows 8"
- select "Change high DPI settings" button
- check "Override high DPI scaling behaviour. Scaling performed by:"
- choose "System (Enhanced)"

Having spent time playing with the k-mix I'm highly impressed with all aspects of it's quality and flexibility. For example, not only is it highly configurable, but control can be achieved via MIDI for automation. I'd highly recommend it as both a small mixer and digital interface. I've not tried the midi pad controller aspect yet. The only small thing I miss is a display of the levels but to be honest that isn't actually necessary when using recording, only when standalone mixing where gain structure matters. Still If you avoid clipping, which is shown on the channel lights, you'll be good.

Oh, another tip. The K-mix can get it's power from two sources, so plugging in a wall wart supply to the second socket when the k-mix is connected to a PC means that when the PC turns off the K-mix stays powered up and keeps it's settings. Of course, you can turn off the k-mix to save the settings too.