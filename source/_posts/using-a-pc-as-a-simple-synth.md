---
title: Using a PC as a simple synth with a MIDI controller
date: 2021-10-22 12:01:00
tags:
---

## The simplest way to play sounds with a MIDI keyboard controller connected to a Widows PC

[Update 2010/10/30] Added using Musecore music notation program. See below.

Now and then I want to play notes on a keyboard to help with my double bass practice. For example, to hear intervals in order to sing them and to check my intonation. Also a keyboard is often the most natural way to visualise scales and harmony. So I required a simple way to play notes and perhaps chords with a keyboard.

One solution would be to use a simple on-screen virtual piano or syth. That's great if you also have a touch screen, which I do.

However I already have an Akai LPK25 25 key mini (micro) keyboard MIDI controller and liked the idea of having real keys, even though they take up desk space. And that's despite the fact the LPK25's keys are tiny and feel horribly springy compared to a piano or weighted controller keyboard. MIDI controllers produce no sounds, rather they just send MIDI messages and a MIDI syth or other sound generator is required to make any sounds

![AKAI LPK25](/images/AKAI-LPK25.jpg)

## Solution

Like most modern MIDI controllers, the LPK25 connects to a PC using USB midi which makes life simpler than the old MIDI 5 pin DIN connectors. It also sends on MIDI ch 1 by default so should be fine in any configuration. AKIA provide a little settings editor but I found it was unecessary in this case. 

The setup steps are:

- Connect the keyboard to the laptop using a USB cable
- Optionally install a MIDI Synth to make the sounds. Windows has long shipped with one but it's truely aweful
- Virtually connect the keyboard to the synth via software

The MIDI Synth I used is [CoolSoft's Virtual Midi Synth](https://coolsoft.altervista.org/en/virtualmidisynth) which requires you to also [install a SoundFont](https://coolsoft.altervista.org/en/virtualmidisynth#soundfonts). I used the recommended **FluidR3_GM** General Midi font as I've used it before and found it to be good enough.

To connect the keyboard to the synth in the PC via software I used the venerable [MIDI-OX](http://www.midiox.com/) midi toolkit which works fine on Windows 10. MID-OX also allows viewing and manipulating MIDI messages if you are so inclined. Once installed, you link up the controller to the synth by selecting the input an output dvices with **Options -> Midi Devices**.

That's it. As long as MIDI-OX is running any key press should be heard in your audio output. The default GM voice is **1 Acoustic Grand Piano** which is fine for me. Note though that with such a simple non ASIO driver set up there is a small but noticable latency.

## Alternative Solution - using Musescore

Thanks to Arash Zargamy for pointing out that the free and open source [Musescore](https://musescore.org/en) notation and compostition program will also act as a simple MIDI synth. There's no need to install an extra synth or route the midi as muscore includes a synth and picks up the MIDI input device.

Just install the musescore programm and play notes on the keyboard. You might need to check `edit -> preferences -> I/O` to ensure your keyboard is the MIDI input device. The default voice is GM piano.

An advantage of Muscore is you can also easily record the dots for your platying. Just click the note input icon on the left of the note toolbar.