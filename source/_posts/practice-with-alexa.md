---
title: Practicing with Alexa and a PC
date: 2023-08-14 12:01:00
tags:
---

***A way to control music practice tools on Windows using Alexa. Initially controlling Song Master playback of a backing track.***

## The concept

For sometime now I've been thinking about using Amazon Alexa voice control for music practice. At a minimum voice activation might make stopping and starting play-along-tracks on a PC easier than my current use of computer keyboard or USB foot switch with Transcribe!. The double bass is big, awkward and risky to manoeuver. Going further I wondered what other options might be available such as controlling any program or interacting with my music practice webs apps. Echo devices with a screen include a web browser so that might give another deployment option too.

For it to work we'd need an Alex skill to recognise utterances and remotely action intents by triggering a server program running on PC and run PC commands to do what ever is required. That's quite a bit of work. In addition to the coding, local configuration would probably be required for the internet router and Windows firewall to allow triggers to come in from outside (maybe webSockets could help simplify). If only the Windows Alexa App had been extended to provide customisable local control to replace the lackluster Cortana, but that never happened.

There is the venerable accessibility program Dragon Naturally Speaking. It's been a long time since I set it up but then it was much better at dictation than computer control. And of course like all access technology, it's expensive.

So I was pleased to find TRIGGERcmd a "cloud service that allows you to securely and remotely run commands on your computers. You install a small server on the PC and commands can be triggered remotely over the interwebs in various ways, including with a supplied Alexa skill.

It turned out pretty straightforward to get basic Alexa control of playback of a backing track in Song Master.

# Song Master

Song Master is a relatively new practice program with a lot of functionality. I recently discovered it when Brent Vaartstra of Learn Jazz Standards showcased it. It's similar to the well loved Transcribe! allowing slowdown and pitch shifting. But like Moises it uses AI to analyses the music to find beats, bars, tempo, pitches, chords,section labels and the key. Also rather than relying on EQ to hone in on instruments it extracts stems that can be individually controlled in a mixer. It also includes practice oriented tools like a metronome along with phrase and key trainers which auto level up. The main developer John Schnurrenber of Aurally Sound is very responsive too which is always important to me.

Like Transcribe!, Song Master has excellent keyboard control, meaning we could use venerable AutoHotKey to automate it. But what makes Song Master even more interesting for Alexa control is it has remote control with midi or OSC (Open Source Control). In particular OSC is ideal for programmatic control in a generic software and internet context. BTW Transcribe! has scripting and that might provide an alternative mechanism for control, if it can be triggered and parameterised.

## "Alexa ask Trigger Command to run toggle playing"

My overall impression of Trigger command is good. It lacks a little professional polish. For example the docs are rather hidden but I easily figured it out. It appears to be a lone developer so the SLA is unknown, but a forum is provided. The free version is limited ot one trigger a minute which is hopeless for debugging and limiting for practice. However the subscription is cheap enough.

Several Alexa skills are provided. Three provide differing skill invocation names: "Trigger Command", "Triger C M D" and "TC" and alow a single parameter to be passed with "Alexa ask TC to run XXX with parameter YYY". The other uses Alexa home automation to create a virtual device per command that can be turned on and off by voice so that the command gets a parameter value of "on" or "off".

The Windows agent is configured using a text editor (a bit flakey) or a GUI. The voice name to run from Alexa is defined as is the command to run (this appears to be passed to Windows cmd). A somewhat convoluted mechanism is also provided to return speech to Alexa. These are 'voice' and 'command' in this config originally set using the GUI

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

This works pretty well and even my Amazon Echo Spot (no longer made) works OK with its limited microphone.

### The sendosc command - The Techy Bit

The OSC specification is just a data format and ech command contains:

- the command as a path: '/togglePlaying'
- a list of parameters of type and value: 'i 1'

The Song Master OSC command reference contains the details of all the available commands. Note that in this case I found I had to pass an integer of 1 even though the reference says no parameter is required (as I would expect).

OSC doesn't specify how the commands are transported but Song Master is running a TCP/IP server that accepts UDP packets on the configured port 8000. So we just need some code to to send the correct command to Song Master on local host 127.0.0.1.

I found a suitable program [sendosc]https://github.com/yoggy/sendosc) with MIT licence. This in turn uses some C++ OSC pack/unpack classes [oscpack](http://www.rossbencina.com/code/oscpack) also MIT licenced. A Windows binary is provided but for safety and future extensibility I prefer to build such things myself, which was straight forward.

The C++ command is fine and could possibly be extended to send Song Master text responses back to Alexa to be spoken if they are synchonous. However I get the impression Song Master may send them asynchronously so a server will be required. Fortunately it looks like the Nodejs classes for OSC are great quality.

### Possible Next Steps

There are other useful Song Master commands to use and perhaps even other programs to control with Alexa. Maybe using AutoHotKey.

Control of soloing and muting bass is a good next step for Song Master. Here we hit a limitation of TRIGGERcmd, it only supports a single parameter and Song Master needs two: mixer channel number and state (on or off). So we need to get creative to hack a solution. The simplest would be to only support channel 1 and always make that bass to be soloed or muted.

Of course if we wrote our own skill and possibly web service we could overcome all the limitations. But we'd then need to solve the extra complications mentioned at the start of this post and that TRIGGERcmd addresses for us (it appears to use webSockets from the web service to the Windows agent given some Python code that is supplied).

## Summary

It is certainly possible to get Alexa to control music practice tools running on a PC. It just required the TRIGGERcmd service and a small open source Windows program.

In fact it works well enough for hands free control though the spoken utterances are a little long winded. Perhaps Alexa routines might help make them shorter.