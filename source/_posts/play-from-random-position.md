---
title: Play Audio Files From Random Position
date: 2021-10-05 12:01:00
tags:
---

I'm currently practicing a Bb Jazz blues ready for my first Jazz jam with an audience.

It's going well but an issue I'm facing is getting back to the correct place in the form after making a mistake or drifting off from being in the moment. I find my mind jumps at any excuse to get in the way of playing.

My strategy is twofold:

- practicing getting back into the moment as soon as I make a mistake or realise I've drifted off. This involves developing a recovery strategy. For me that's means taking a breath, feeling the strings and thinking about the current notes. Mistakes are harder to recover from as you have to avoid an emotional response and then forget about the past mistake,
- practicing figuring out where in the form or chord sequence you are given a random starting place. This requires someone else to manage the volume and brining it up at a random time or doing it yourself. Both approaches allow estimation of position by counting beats which rather reduces the effectiveness

I decided to try automating the second approach. Ideally I'd use iReal Pro as this provides a great varying backing track based on the chords. However, it doesn't offer any programability and I'm not aware of Android scripting tools. In addition, I run in in the Blues Stacks Android emulator on Windows, adding more complexity. Once Windows 11 runs Android apps directly the configuration will be less complex.

So I decide to create something to play audio files starting from a random position. That will work with any audio files I already have on hard disc, ripped CDs, youtube conversions, downloads and even exported audio from iReal Pro.

I wanted a quick, low code solution. Exploring the options I decided to create a `cmd` script using the windows ports of the powerful `ffmpeg` tools to do the work. It's possible to drag files onto a cmd icon (or a shortcut to one) thus giving a more GUI way of working than command line.

Here's the script.

``` cmd
@echo off
rem Plays an audio file froma random starting position
rem requires ffprobe and ffplay from ffmpeg https://ffmpeg.org/
rem drag audio file onto this script or a shortcut to it

setlocal
set _sdir=%~dP0
set _file=%1

rem seconds increment
set _incr=3

rem calculate a random starting position 
rem need to escape parenthesis in filenames to avoid problems with for /f
rem need " or else the = are lost
rem /a does a floor but with a "Missing operator." message
rem can't use %random% as it is seeded every cmd run with the time so hardly varies at all
set _efile=%_file:(=^(% 
set _efile=%_efile:)=^)%
for /f "usebackq tokens=*" %%a in (`"%_sdir%ffprobe -v error -show_entries format=duration -i %_efile% -of default=noprint_wrappers=1:nokey=1"`) do set _dura=%%a
set /a _dura=_dura
for /f "usebackq tokens=*" %%a in (`PowerShell.exe -command "Get-Random -Maximum %_dura%"`) do set _rand=%%a
set /a _start=(%_rand%/%_incr%*%_incr%)

rem play from starting position
%_sdir%ffplay -v error -i %_file% -autoexit -ss %_start%
```

It uses `ffprobe` to get the duration of the file in seconds. Then it generates a random number of seconds in the audio which it uses as the starting position for `ffplay`.
 
To deploy, the script file is put in a folder along with the `ffplay` and `ffprobe` windows exes downloaded from [ffmpeg.org](https://ffmpeg.org/). A Windows shortcut on the desktop references the script and is the drag and drop target.

This works OK, at least for now. The random generator doesn't seem ideal so I might change it at some point. Also the `_incr` value probably needs tuning better to compromise between not being too small and giving a good variation within repeating chord patterns.