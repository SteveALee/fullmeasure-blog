---
title: Using a PC as a Simple Synth
date: 2021-10-22 12:01:00
tags:
---

Now and then I want to play notes on a keyboard to help with my double bass practice. For example, to hear intervals in order to sing them and to check my intonation.

One solution would be to use a simple on-screen virtual piano or syth. That's great if you also have a touch screen.

However I already have an Akai LPK25 25 key mini (micro) keyboard and liked the idea of having real keys, even though they take up desk space. 
And that's despite the fact the LPK25's key are tiny and feel horribly springy compared to a piano or weighted controller keyboard. 

![AKAI LPK25](/images/AKAI-LPK25.jpg)

It's also possible to use full DAW like Reaper but that's a lot of extra complexity that's totally unneeded and would no doubt be a big distraction.

## Solution

Like most moder controllers, the LPK25 connects to a PC using USB midi which makes life simpler that the old MIDI connectors. It also sends on MIDI ch 1. AKIA provide a little settings editor but I found it was unecessary in this case. The setup steps are.

- Connect the keyboard to the laptop using a USB cable (mini plug at AKAI end)
- Install a MIDI Synth to make the sounds. Windows has long shipped with one but it's truely aweful
- Virtually connect the keyboard to the synth

The Midi Synth I used is [CoolSoft's](https://coolsoft.altervista.org/en/virtualmidisynth) and it requires you to also [install a SoundFont](https://coolsoft.altervista.org/en/virtualmidisynth#soundfonts). I used the recommended FluidR3_GM General Midi font as I've used it nbefore and its good enough.

To connect the keyboard to the synth I used the venerable [MIDI-OX](http://www.midiox.com/) midi toolkit which works fine on Windows 10. One installed you select the input an output dvices using `Options -> Midi Devices`.

That's it. As long as MIDI-OX is running any key press should be heard in your audio output. The default GM voice is Piano which is fine for me. Note with such a simple non ASIO driver set up there is a small but noticable latency.