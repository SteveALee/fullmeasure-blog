---
title: Practicing with Alexa and a PC
date: 2023-08-14 12:01:00
tags:
---

***A way to voice control music practice tools on Windows using Alexa. Initially controlling Song Master playback of a backing track.***

## The concept

For sometime now I've been thinking about using Amazon [Alexa](https://developer.amazon.com/en-GB/alexa/) voice control for music practice. At a minimum, voice activation would make stopping and starting play-along-tracks on a PC easier than my current use of computer keyboard or USB foot switch with [Transcribe!](https://www.seventhstring.com/xscribe/overview.html). The double bass is big, awkward and risky to manoeuver. Going further, I wondered what other options might be available. For example, controlling any other music program or interacting with my music practice webs apps. Finally, Echo devices with a screen include a web browser of sorts so that might give another deployment option for my practice web apps.

For Alexa control to work we'd need an Alex skill to recognise utterances and remotely trigger a program running on PC that will run PC commands to do what ever is required. That's quite a bit of development work. In addition to the coding, local configuration would probably be required for the internet router and Windows firewall to allow triggers to come in from outside. If only the Windows Alexa App had been extended to provide customisable local control to replace the lackluster Cortana, but that never happened.

As an alternative there is the venerable Windows accessibility program Dragon Naturally Speaking. It's been a long time since I set it up but even then it was much better at dictation than computer control. And of course like all access technology, it's expensive.

So I was pleased to find the [TRIGGERcmd](https://www.triggercmd.com/en/) "cloud service that allows you to securely and remotely run commands on your computers". You install a small server on a PC (or other device) and commands can be triggered remotely over the interwebs in various ways, including with a provided Alexa skill.

It turned out pretty straightforward to get basic Alexa control of playback of a backing track in the [Song Master](https://aurallysound.com/) "musician's learning assistant" program. This should scale and also work on platforms other than Windows

## Song Master

Song Master is a relatively new practice program with a lot of functionality. I recently discovered it when Brent Vaartstra of [Learn Jazz Standards](https://www.learnjazzstandards.com/) showcased it. It's similar to the well loved Transcribe!, allowing slowdown and pitch shifting for learning by ear. But like Moises it uses AI to analyse the music to find beats, bars, tempo, pitches, chords, section labels and the key. Also rather than relying on EQ to home in on specific instruments it extracts stems that can be individually controlled in a mixer. It also includes practice oriented tools like a metronome along with phrase and key trainers which auto level up. The main developer John Schnurrenber of Aurally Sound is very responsive too which is always important to me.

Like Transcribe!, Song Master has excellent keyboard control, meaning we could use venerable [AutoHotkey](https://www.autohotkey.com/) to automate it. But what makes Song Master even more interesting for Alexa control is it has remote control features with MIDI or OSC (Open Source Control) connectivity. In particular, OSC is ideal for programmatic control in a software and internet context. BTW Transcribe! has scripting and that might provide an alternative mechanism for control, if it can be triggered and parameterised.

## "Alexa ask Trigger Command to run toggle playing"

So the goal is to start and stop playing in Song Master with a voice Command via its OSC support. TRIGGERcmd can provide the Alexa skill and system integration. I then only need to create a way to talk OSC to Song Master

My overall impression of Trigger command is good. Maybe it lacks a little professional polish. For example the docs are hidden, requiring a web search. But I easily figured it out and configured it as I wanted. It appears to be a one man team behind it so the SLA is unknown, but size of company is no guarantee of stability or support :D. 

The free version is limited to one trigger a minute which is hopeless for debugging and limiting for practice. Debugging largely a matter of locally invoking commands and the subscription is cheap enough anyway.

Several Alexa skills are provided. Three provide differing skill invocation names: "Trigger Command", "Trigger C M D" (I had to work that out) and "TC". They alow a single parameter to be passed with "Alexa ask TC to run XXX with parameter YYY". The other skill uses Alexa home automation to create a virtual device per command that can be turned on and off by voice so that the command gets a parameter value of "on" or "off". Perhaps other parameter types will eventually become available, like percentage.

The Windows agent is configured using a text editor (kept locking up) or a GUI. For each command, the voice name to run from Alexa is defined as is the Windows command to run. The Windows command appears to be passed to cmd in a new shell, as you'd expect. A somewhat convoluted mechanism is also provided to return speech to Alexa via a HTTP post to a TRIGGERcmd endpoint using curl (not part of Windows).

In the following config 'voice' and 'command' are the two main values. The othees were set by the GUI editor when creating the command.

```json
 {
  "trigger": "Toggle playing",
  "command": "C:\\projects\\sendosc\\build\\Release\\sendosc 127.0.0.1 8000 /togglePlaying i 1",
  "offCommand": "",
  "ground": "foreground",
  "voice": "toggle playing",
  "voiceReply": "",
  "allowParams": "false"
 }
 ```

The result works well and even my Amazon Echo Spot (no longer made) works well enough  with its limited microphone when the music is playing.

## The "sendosc" Command - The Techy Bit

So we need a command to invoke Song Master's OSC support with the correct parameters.

The [OSC specification](https://opensoundcontrol.stanford.edu/) is a message data format and each message contains binary data:

- the address as an ASCII encoded path: '/togglePlaying'
- a list of parameters of type and value: 'i 1'

The [Song Master OSC command reference](https://aurallysound.com/blogs/quick-start/osc-commands) contains the details of all the available commands it can send or receive. Note that in this case I found I had to pass an integer of 1 even though the reference says no parameter is required (as I would expect).

OSC doesn't specify how the commands are transported (as clarified in version 1.1) but Song Master is running the common pattern of a TCP/IP server that accepts UDP packets on the configured port (8000). So we just need some code to to take the command passed as a parameter, format it according to OSC and send the result as a UDP packet to Song Master on localhost 127.0.0.1.

I found a suitable program [sendosc](https://github.com/yoggy/sendosc) with MIT licence. This in turn uses some C++ OSC pack/unpack and classes with minumal UDP network classes [oscpack](http://www.rossbencina.com/code/oscpack) also MIT licenced. A Windows binary is provided but for safety and future extensibility I prefer to build such things myself, which was straightforward using ```cmake``` and ```Visual Studio Community 2022``` (free version).

The compiled C++ command works just fine but could be improved to handle errors and return OSC server responses (via stdout I guess).

## Possible Next Steps

There are other useful Song Master commands to use and perhaps even other programs to control with Alexa. Maybe using AutoHotKey.

Control of soloing and muting bass is a good next step for Song Master. Here we might hit a limitation of TRIGGERcmd, it only supports a single parameter and Song Master needs two: mixer channel number and state (on or off). So we need to get creative to hack a solution. The simplest would be to only support channel 1 and always make that bass to be soloed or muted.

We could return Song Master OSC responses back to Alexa to be spoken. I'm not yet sure if Song Master sends responses synchronously or asynchronously so a server may be required. Fortunately it looks like the Nodejs classes for OSC are of excellent quality.

Of course if we wrote our own skill and possibly a web service we could overcome all the limitations. But we'd then need to solve the extra complications mentioned at the start of this post and that TRIGGERcmd addresses for us (it appears to use webSockets from the web service to the Windows agent given some Python code that is supplied).

## Summary

It is certainly possible to get Alexa to control music practice tools running on a PC. It just required the TRIGGERcmd service and a small open source Windows program.

If many commands are required the maintenance could get to be a bit of a handfull. Though a batch file could be used to abstract some of the common code. Having a text file makes life much easer of course (assuming we can edit in our own tool and cause a refresh after saving).

It works well enough for hands free control when practicing, though the spoken utterances are a little long winded. Perhaps Alexa routines might help make them shorter.
