---
title: Music Practice with Alexa and Song Master
date: 2023-08-28 12:01:00
tags:
---

*** Voice control of Song Master for music practice using Alexa ***

In the [previous post](../../14/practice-with-alexa) I introduced the concept of using Alexa to control Song Master for hands free music practice. Since then I've built out a solution that covers all my basic requirements with room to expand.

This involved digging deep into the technologies behaviour and creating the following:

- `cosc.exe` command line tool based on sendosc which also displays received OSC packets
- `songmaster.cmd` script to abstract Song Master OSC commands for easy command line use
- `commands.json` configuration file for TRIGGERcmd use with Alexa skills

These items are available on GitHub in the [music-practice-tools/cosc](https://github.com/music-practice-tools/cosc) repository, should you want to play. I'll soon add a tutorial to explain how to install the various parts.

## In C

The Windows CLI executable `cosc` sends OSC packets in the same way as `sendosc`. It also receives and displays packets for a specified time or until a ctrl-C. It's named for similarity to old war horse program `curl`, with 'c' meaning 'see'.

It's been over 30 years since I last did some serious C++ programming but fortunately it was like riding a bike. Visual Studio seemed like an old friend as it's really not changed much. I took the simple architectural approach of starting to receive as soon as we have sent. While this introduces a small potential race condition where 'response' messages might be missed, it works fine in practice and avoided more complex multi-thread code.

I was pleased to find the versions of C++ supported by Visual Studio Community 2022 include lambda functions. These allows functional programming (FP) which made the code much easier (and with far less boilerplate the OOP). The lambda syntax is straight forward and the "functional" standard library allows them to be easily passed to functions (and returned) as first class data, a basic feature of FP called higher order functions. Note, pointers to functions are really not the same thing, though similar in some ways. It's nice to see the C++ language evolving.

I found being able to receive OSC packets from Song Master to be invaluable for learning and debugging. Currently though, Song Master messages are not fed back to Alexa, as explained below.

## Still Crazy After All the Years

I have kept my hand in with Windows cmd scripting by maintaining a script I created to build a version of Audacity with ASIO support on Windows. It's still a maddening thing to develop with, having so many half baked features and nasty gotchas guaranteed to waste time. I probably should have learnt PowerShell which is now portable but I never liked the syntax. Fun fact: the chief maintainer of PowerShell has the same name as me. Give me clunky old bash script any day.

The `songmaster.cmd` script simply provides a slightly abstracted set of commands that use `cosc` to send OSC messages to Song Master. Here they are:

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

The parameters have been chosen to for simplicity and also to match the way TRIGGERcmd invokes them. There's room for some improvement yet.

## Commands.json

This file invokes configures TRIGGERcmd to invoke the `songmaster.cmd` script with suitable parameters according to how it passes on Alexa parameters from the provided skills. This needs a little explanation.

TRIGGERcmds provides two types of Alexa skill. The Smart Home skill and the basic skill with several aliases. Of the two types, the Smart Home skill affords easier voice control, but has limitations with possible parameters.

The Smart Home skill adds an Alexa device for each command defined in `commands.json`. The advantage of this is that devices have a predefined set of actions that can easily be invoked by speaking to Alexa to cause parameters to be passed to the script.

- "on"
- "off"
- a positive number - I've found no way to specify negative
- a percentage - converted to a number between 0 and 100
- a colour - no use to us here

There may be more to discover buried in the Alexa dev documentation.

The main "command" field of a command definition in TRIGGERcmd is invoked for "on". The contents are simply passed to cmd. For "off" the "offCommand" field is invoked, again with no parameter. The other values are passed as a parameter at the end of the field contents.

Many of the Song Master commands can thus be invoked with incantations such as:

- "alexa turn mac play on" - runs "songmaster"
- "alexa set mac speed 80" - runs "songmaster 80"

Where "turn" and "set" are optional.

Note I named the devices "mac *something*" where "mac" stands for "music access". This can get confused with "max" so I might revisit this choice.

Commands with multiple parameters or named parameters not supported by Alexa Smart Home have to be invoked with the other Alexa skill. Like this:

- "alexa ask TC to run mac goto with parameter next section"

Where "next" and "section" are passed to the TRIGGERcmd command as parameters. Rather a mouthful and a touch unreliable compared to the Smart Home device invocations. Unfortunately Alexa keeps hearing "perimeter" even though I've been providing feedback in the Alexa app.

I hope something might improve here. Ie custom Smart Home skill parameter lexicons that can be configured in commands.json. We could use numbers (or colours) to indicate named parameter values but that would be just too much of a cognitive load for me.

To get Alexa to speak a response, TRIGGERcmd supports a "Voice Reply" field. This can also be set to the contents of a file that is filled in by the command. But this is not available for the Smart Home commands which always just speak "OK". I wish this could be turned off for individual commands while others always spoke the response text. The only Alexa option is to make a noise instead of speaking anything, including "OK".

But for now the common actions I require are mostly available with the simpler invocation style. And the longer style can be spoken for the others. I do plan to revisit the speech design and defaults in the hope of smoothing it out a bit.

```cmd
alexa mac play { on off }
alexa mac speed { 20 40 50 60 70 80 90 100 120 } number or percent
alexa mac up { 0...n }
alexa mac down { 0...n }
alexa mac mute { on off } { 1 - 5 }
alexa mac solo { 1 - 5 off }
alexa mac metronome { on off }
alexa mac loop { on off } { 0=note 1=bar 2=section }
alexa ask tc to run mac loop with parameter { bar note section }
alexa mac advance { 0=note 1=bar 2=section 100=end }
alexa mac retreat { 0=note 1=bar 2=section 100=start }
alexa mac setlist { on 1...n } on = next
alexa ask tc to run mac setlist with parameter { 1...n info songs next previous }
```
