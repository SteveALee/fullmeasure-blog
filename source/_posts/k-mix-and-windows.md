---
title: K-mix and Windows
date: 2020-03-27 16:47:00
tags:
---

[[Skip to Work-around for Editor issues on Windows](#Work-around-for-Editor-issues-on-Windows)].

## Keith McMillen Instruments k-mix

Watching [Steve Lawson playing](https://youtu.be/l2zWzpvhOVQ?t=24) you may notice a couple of interesting devices in his setup with prominent white shapes on a black background. These are the Keith McMillen Instruments (KMI) [k-mix](https://www.keithmcmillen.com/products/k-mix/) mixer, digital interface plus control surface and the [QuNeo](https://www.keithmcmillen.com/products/quneo/) pad controller. The white areas are KMI's touch 'fabric' which provides unique touch control dynamics.

Since giving my old Spirit Folio Notepad mixer to the charity I work for that records news for the blind I have occasionally required a small mixer suitable for connecting a Rode NT-1 condenser mic and bass guitar to a Windows PC for recording.

In addition, I'd like to get creative with digital music using a DAW and flexible digital interface for bass vox and pads. Last time I messed around with stuff like this was in the distance past on an Atari.

Having chatted to Steve, it's clear he holds the McMillen products in high regard and the K-mix sounded unique in its capabilities and flexibility. It's also a decent price and nice and compact too. Windows support is now available in the form of a Windows version of the editor and an ASIO driver. While I do have Apple products I've never used them much, sticking to Windows, Linux and Android for my tech platforms

![Laney KB80 combo](/images/k-mix.jpg)

## Getting mixxy with it

My first task was to record a voice over for a video introducing a cognitive accessibility project I'm working on. This was a very simple setup: mic XLR into the k-mix input 1 with it's excellent pre amp, 48v phantom power on, a monitor amp on output 1 and USB lead to PC with Audacity to record channel 1.

It didn't quite work out as I expected, the level was very low. But it turns out that was my not understanding the default config. I was expecting the USB channels to mirror the outputs, at least for ch 1 & 2. But that's not how the k-mix is configured and on reflection that's due to the more common use cases. As a stand alone digital mixer with analogue inputs and outputs, it works beautifully as you'd expect. However as a digital interface the default settings bypass  the mixer controls, sending pre fader feeds to USB. However the channel gain (trim as it's called) is pre the feed so does have effect on the level. By adjusting that I was able to get a decent recording level and complete the job.

Of course as you'd hope, the k-mix lets you set the digital usb feed to be post fader. This setting, like a few others can only be changed using the K-Mix editor program. And this is where I hit a blocking problem. The editor would not run for me. However talking to Evan at McMillen, it turns out a recent update to Windows seems to have caused the problems I was experiencing. The issues were: Editor would not run as it got stuck in an upgrade loop from from 1.3.5 to 1.3.5 and the windows was so small the text was clipped.

## Work-around for Editor issues on Windows

While McMillen work on a fix to the problems caused by the Windows update, there's a temporary work around using Windows' compatibility feature as follows:

- right click on "K-Mix Editor.exe" in "Program files".
- select "Properties"
- select the "Compatibility" tab at the top
- check "run this in compatibility mode for:"
- choose "Windows 8"
- select "Change high DPI settings" button
- check "Override high DPI scaling behaviour. Scaling performed by:"
- choose "System (Enhanced)"

## Vinyl ripping

My second task has been to record some vinyl records by connecting up my Dual 505 record deck via a home made RIAA preamp. Again this worked really well after some experimentation as I learnt the k-Mix features. Inputs 3 & 4 are a good choice as they are less sensitive than 1 & 2. Setting 3 & 4 as a stereo pair pans them to the main 1 & 2 bus as required by simpler PC recording software like audacity which only use these channels (ie stereo in). The last step was to set inputs 1 & 2 to be post fader so the PC sees them. It seems the k-mix also supports the RIAA curve equalisation so I'll give that a go, directly connecting the Ortofon moving coil cartridge output to the k-mix. That should give less noise than my old pre amp.

## K-mix as a sound card

I also use the K-mix as a sort of high quality external sound card, connecting Outputs 1 & 2 to my ancient NAD amplifier. The sound quality is excellent. I briefly tried using a USB 3.0 extension cable but the sound was awful; the unbranded, unknown quality cable is going right back to Amazon. Another problem with the current driver / Windows Update combination is that about half of the Windows media player programs I tried currently crash. VLC, Groove, WinAmp all fail to play but Transcribe, Firefox and Reaper al all good. Again, KMI are looking into this.  

## Summary thoughts and a power tip

Having spent time playing with the k-mix I'm highly impressed with all aspects of its quality and flexibility. For example, not only is it highly configurable, but control can be achieved via MIDI for extensive automation. I'd highly recommend it as both a small mixer and digital interface. I've not tried the midi pad controller aspect yet. The only small thing I miss is a display of the levels but to be honest that isn't actually necessary when using recording, only when standalone mixing where gain structure matters. Still If you avoid clipping, which is shown on the channel lights, you'll be good.

On final tip. The K-mix can get its power from two sources, so plugging in a wall wart supply to the second socket when the k-mix is connected to a PC means that when the PC turns off the K-mix stays powered up and keeps its settings. Of course, you can turn off the k-mix to save the settings too.
