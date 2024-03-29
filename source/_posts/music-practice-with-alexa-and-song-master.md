---
title: Music Practice with Alexa and Song Master
date: 2023-08-28 12:01:00
tags:
---

*** Voice control of Song Master for music practice using Alexa ***

In the [previous post](../../14/practice-with-alexa) I introduced the concept of using Alexa to control Song Master for hands free music practice on Windows. Since then I've built out a solution that covers all my basic requirements with room to expand.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zSoyVTqE1ck?si=mA3ko-2wnRklpVQv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

This involved digging deep into the various technologies behaviour and creating the following:

- `cosc.exe` command line tool based on sendosc which also displays received OSC packets
- `songmaster.cmd` script to abstract Song Master OSC commands for easy command line use
- `commands.json` configuration file for TRIGGERcmd use with Alexa skills

These items are available on GitHub in the [music-practice-tools/cosc](https://github.com/music-practice-tools/cosc) repository, should you want to play. 

Setup instructions are located at the end of this post.

## In C

The Windows CLI executable `cosc` sends OSC packets in the same way as `sendosc`. It also receives and displays packets for a specified time or until a ctrl-C. It's named for similarity to old war horse program `curl`, with 'c' meaning 'see'.

It's been over 30 years since I last did some serious C++ programming but fortunately it was like riding a bike. Visual Studio seemed like an old friend as it's really not changed much. I took the simple architectural approach of starting to receive as soon as we have sent. While this introduces a small potential race condition where 'response' messages might be missed, it works fine in practice and avoided more complex multi-thread code.

I was pleased to find the versions of C++ supported by Visual Studio Community 2022 include lambda functions. These allows functional programming (FP) which made the code much easier (and with far less boilerplate the OOP). The lambda syntax is straight forward and the "functional" standard library allows them to be easily passed to functions (and returned) as first class data, a basic feature of FP called higher order functions. Note, pointers to functions are really not the same thing, though similar in some ways. It's nice to see the C++ language evolving.

I found being able to receive OSC packets from Song Master to be invaluable for learning and debugging. Currently though, Song Master messages are not fed back to Alexa, as explained below.

For simplicity I only built a Windows version by creating a Visual Studio solution. The original `sendosc` and `oscpack` both use `cmake` to also build for Linux and Mac and it should be possible to adapt my code to do the same,  

## Still Crazy After All these Years

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

## Do Nothin' Till You Hear from Me

The `commands.json` file configures TRIGGERcmd to invoke the `songmaster.cmd` script with suitable parameters according to how Alexa parameters are passed from the skills from the provided skills. This needs a little explanation.

TRIGGERcmd provides two types of Alexa skill. The Smart Home skill and the basic skill with several aliases. Of the two types, the Smart Home skill affords easier voice control, but has limitations with possible parameters.

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

Where "next" and "section" are passed to the TRIGGERcmd command as parameters. Rather a mouthful and a touch unreliable compared to the Smart Home device invocations. Unfortunately Alexa keeps hearing "perimeter" even though I've been providing feedback in the Alexa app. Fortunately, the TRIGGERcmd developer has offered to add "argument" as an alternative.

I hope something might improve here. Ie custom Smart Home skill parameter lexicons that can be configured in commands.json. We could use numbers (or colours) to indicate named parameter values but that would be just too much of a cognitive load for me. I have added numeric alternatives for a couple commands which have an enumerated parameter but I'm not happy with it.

To get Alexa to speak a response, TRIGGERcmd supports a "Voice Reply" field. This can also be set to the contents of a file that is filled in by the command. But this is not available for the Smart Home commands which always just speak "OK". I wish this could be turned off for individual commands while others always spoke the response text. The only Alexa option is to make a noise instead of speaking anything, including "OK".

But for now the common actions I require are mostly available with the simpler invocation style. And the longer style can be spoken for the others.

Here are the current Alexa invocations:

```cmd
alexa mac play { on off }
alexa mac speed { 20 40 50 60 70 80 90 100 120 } number or percent
alexa mac up { 0...n }
alexa mac down { 0...n }
alexa mac mute { on off } { 1 - 5 }
alexa mac solo { 1 - 5 off }
alexa mac metronome { on off }
alexa mac loop { on off } { 0=bar 1=note 2=section }
alexa ask tc to run mac loop with parameter { bar note section }
alexa mac advance { 0=bar 1=note 2=section 100=end }
alexa mac retreat { 0=bar 1=note 2=section 100=start }
alexa mac setlist { on 1...n } on = next
alexa ask tc to run mac setlist with parameter { 1...n info songs next previous }
```

## Alexa Use

Alexa runs on many devices and I've used an obsolete Echo Spot, a new Echo Flex and the Android App. The obvious requirement is that Alexa can easily hear you when music is playing so you might need to work on positioning. I've had reasonable success with practice volume levels including when playing along to music.

Perhaps a successful approach would be to use the phone app with a wireless mic such as a bluetooth headset that can be close to your mouth. Assuming the Alexa app will work with any audio input device. If your "instrument" is singing you might have some fun.

I do plan to revisit the speech lexicon design and defaults in the hope of smoothing it out a bit. But for now, the chosen words seem to work fairly reliably without Alexa getting excited and triggering built in actions like playing on Amazon music, which it is horribly keen to do.

## Latency

As both TRIGGERcmd and Alexa are both web services hosted at unknown geographic locations there is latency. I assume the speech recognition is server based rather than running in the Alexa device so that will be a delay. Then the service has to invoke the Skill code, which then triggers the command running on the PC. This could only be avoided if the PC version of Alexa did local voice recognition and could invoke local commands. I'm not holding my breath for that.

I experience a latency of less than 1 second from the end of a command being spoken to action. That has not been a real problem when using in my practice, given the type of interactions. The trick is to set up as much as possible in Song Master at the PC and then use Alexa for simple actions during practice.

## Setup Instructions

- Install [Song Master](https://aurallysound.com/). Ensure Edit External Connections -> Receive OSC is on for port 8000. For debugging Receive OSC can be enabled and SM checked.
- Set up Alexa.
- Signup for [TRIGGERcmd](https://www.triggercmd.com/en/), install the Windows Agent, the TRIGGERcmd Smart Home Alexa skill and possibly the TC skill as described in the [instructions](https://www.triggercmd.com/user/computer/create). You'll need a subscription so you can send more that one trigger a minute.
- Save the TRIGGERcmd [commands.json](https://github.com/music-practice-tools/cosc/blob/main/commands.json) file to `C:\Users\[your-user-name]steve\.TRIGGERcmdData\commands.json`.
- Save the [songmaster.cmd](https://github.com/music-practice-tools/cosc/blob/main/songmaster.cmd) script file to your computer. You'll need to edit TRIGGERcmd `commands.json` to access `songmaster.cmd` where you located it (ie replace all occurrences of `"C:\\projects\\cosc\\songmaster.cmd"`).

Now you can try any of the Alexa commands listed above. Song Master must be Running. The possible Alexa commands can also be worked out from `commands.json`. If you need to debug, try running `songmaster.cmd` with various options from the command line. To see what commands are available run `songmaster usage`. If you still have problems try `cosc` with the commands and in receive mode to view any responses from Song Master.

If you have any questions or suggestions then please feel free to add [issues on the GitHub project](https://github.com/music-practice-tools/cosc/issues).
