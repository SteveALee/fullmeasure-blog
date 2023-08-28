---
title: Practicing with Alexa and a PC Part Two
date: 2023-08-28 12:01:00
tags:
---

***A way to voice control music practice tools on Windows using Alexa. Initially controlling Song Master playback of a backing track.***

In the [previous post](../../14/practice-with-alexa) I introduced the concept of Using Alexa to control Song Master for hands free music practice. Since then I've built out a solution that covers all my basic requirements with room to expand.

This involved digging deep into the technology behaviour and creating:

- `cosc.exe` command line tool based on sendosc but which also displays received OSC packets
- `songmaster.cmd` script to abstract Song Master OSC commands for easy command line use
- `commands.json` to configure TRIGGERcmd for the Alexa skills

These are all available on GitHub in the [music-practice-tools/cosc](https://github.com/music-practice-tools/cosc) repository should you want to play. I'll add a tutorial to simplify installing the various parts.

## In C

The Windows CLI executable `cosc` sends OSC packets in the same way as `sendosc`. It also receives and displays packets for a specified time or until ctrl-C. It's named for similarity to old war horse program `curl`, with 'c' meaning 'see'.

It's been over 30 years since I last did some serious C++ programming but fortunately it was like riding a bike. Visual Studio seemed like an old friend; it's not changed much. I took the simple architectural approach of starting to receive as soon as we have sent. While this introduces a small potential race condition where 'response' messages might be missed, it works fine in practice and avoided more complex multi-thread code.

I was pleased to find the version of C++ supported by Visual Studio Community 2022 includes lambda functions. These allows functional programming (FP) which made the code much easier (and with far less boilerplate the OOP). The lambda syntax is straight forward and the "functional" standard library allows them to be easily passed to functions (and returned) as first class data, a basic feature of FP (higher order functions). Note pointers to functions are not the same thing, though close. It's nice to see the C++ language evolving.

I found receiving OSC packets from Song Master to be invaluable for learning and debugging. Currently though Song Master messages are not fed back to Alexa, as explained below.

## Still Crazy After All the Years

I have kept my hand in with Windows cmd scripting by maintaining a script I created to build a version of Audacity with ASIO support on Windows. It's still a maddening thing to develop with, having so many half baked concepts and nasty gotchas guaranteed to waste time. I probably should have learnt PowerShell which is now portable but I never liked the syntax. Give me bash script anyday, or maybe one day OSs will include Javascipt out of the box.

The script simply provides a slightly abstracted set of commands that map to cosc calls that forward OSC to Song Master. Here they are:

```cmd
play { on off }
speed { 20 40 50 60 70 80 90 100 120 }
pitch { up down key} { octave 1...n }
mute { on off } { 1 - 5 }
solo { 1 - 5 off }
metronome { on off }
loop { on off bar section note track }
goto { start end next previous } [{bar section note }]
playlist { 1...n info songs next previous }
```

The parameters have been chosen to match the way TRIGGERcmd invokes them, partly as a result of Alexa Smart Home skill quirks.

## Commands.json

This file invokes the scriptfile with suitable parameters. It also parses on parameters that come from the Alexa Skills. This needs a little explanation.

TRIGGERcmds provides two types of Alexa skill that can be installed. The Smart Home skill and the basic skill. Of the two types, the Smart Home skill affords easier voice control, but has limited flexibility.

The Smart Home skill adds an Alexa device for each command. The advantage of this is that device have a predefined set of actions that can easily be invoked to cause parameters to be passed to the commands.

- "on"
- "off"
- a positive number - I found no way to make negative
- a percentage - converted to a number between 0 and 100
- a colour - no use to us here

The TRIGGERcmd commands main "command" field is invoked for "On" with no parameters. The other values are passes as a parameter at the end of the command. For off TRIGGERcmd "offCommand" is invoked, with no parameter.

Many of the Song Master commands can thus be invoked with incantations such as:

- "alexa turn mac play on"
- "alexa set mac speed 80"

Where "turn" and "set" are optional.

Note I named the devices "mac *somehting*" where "mac" stands for "music access".

Commands with multiple parameters or other named parameters have to be invoked with the other Alexa skill. Like this:

- "alexa ask TC to run mac goto with parameter next section"

Rather a mouthful and a touch unreliable command to the Smart Home device invocations. I hope something might improve here. Ie custom Smart Home skill parameter lexicons that can be configured in commands.json. Multiple parameters would be useful too. We could use numbers (or colours) for the named values but that would be just too much of a cognitive load for me.

But for now the common actions I require are available with the simpler invocation style.
