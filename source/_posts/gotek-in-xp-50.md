---
title: Using a Gotek USB Floppy Emulator in a Roland XP-50
date: 2023-03-15 16:47:00
tags:
---

Further to my last post on recommissioning my Roland XP-50 workstation, I replaced the floppy drive with a Gotek floppy emulator so I can now use USB memory sticks to store files and transfer between my PC. For example MIDI .mid files can be put on a virtual floppy on a USB via my PC and then accessed by the XP-50.

The [Gotek USB Floppy Emulators](https://www.gotekemulator.com/) are well proven replacements for 3.5 inch floppy drives. They use ubiquitous USB memory sticks to hold a large number of virtual floppies using image files. Replacing the stock firmware with FastFloppy provides a reliable solution that works in almost all possible applications, including old Roland workstations

The WinImage program is useful for management of the images on a PC, including inserting and removing files.

![The Gotek in situ](/images/XP-50/gotek.jpg)
<figcaption>Gotek Floppy Emulator working in the XP-50</figcaption>

## Acquiring a Gotek

Gotek only supply in bulk but there many, many suppliers that will sell to end users. Confusingly, there are many models available from gotek and several variations have appeared over time. Many suppliers to not specify which they provide. It is possible to upgrade the stock Goteks with a better display and a rotory control. Some suppliers provide these updates ready made.

Fortunately, most of this conmplexity is covered in the [FastFloppy Gotek model documentation](https://github.com/keirf/flashfloppy/wiki/Gotek-Models).

I played safe a purchased a device specifically listed as [SFR1M44-U100K GoTek 1.44MB USB SSD Floppy Drive Emulator for YAMAHA KORG ROLAND Electronic Keyboard](https://www.gotekemulator.com/P_view.asp?pid=55) but didn't know about the significant variation over time that FastFloppy details. Fortunately, the model I received was a recent SFRKC30AT3 using the slightly less powerful Artery AT32F415 chip.

## Setting up the Gotek

The first thing to do is Install FastFloppy. The Gotek is the installed replacing the floppy drive.

Installing FastFloppy on the Gotek is straightforward. Connect the Gotek to a PC with a USB cable, install the Artery driver and flashing software and use it to install FastFloppy. Full details provided in the [FastFloppy Firmware Programming docs](https://github.com/keirf/flashfloppy/wiki/Firmware-Programming).

Swapping the drives is also straightforward as you'd expect. In my case 4 screws and after some confusion over jumper settings only a S0 jumper was needed. The floppy was configured for S0 (first drive) and Ready signal on pin 24 but FastFloppy just required the S0. 

After installation, when the XP-50 is turned on the Gotek display shows "F-F" if all is well.

## Testing and Using the Gotek

After some confusion caused by various aged articles, the latest FastFloppy makes it "easy peasy" to use the drive. Both in the XP-50 and transferring files between the XP-50 and a PC. My initial test approach was as follows

- Format the USB stick as FAT using Windows Explorer to DiskManager
- Grab the FastFloppy empty 1.44MB image file from the [image library](https://github.com/keirf/flashfloppy-images/blob/master/README.md) and copy it twice to the stick
- Plug the stick into the Gotek and turn the XP-50 on
- After startup the Gotext buttons allow switching between "000" and "001" indicating the two images are seen
- Use XP-50 Disk utilities to format the image - takes over a minute but then the Gotek is emulating a slow floppy
- Plug the stick into a a PC running WinImage
- Grab a MIDI .mid file and add it to the formatted image file (I used Bach's Air on A G String from MidiWorld)
- Pop the stick back in the XP-50, select the image, load the file and play it

That's it. We can read and write files to the floppy images on both devices.

## Unknowns

Unless I upgrade the Gotek hardware I will need to keep track of the slot numbers for each image file. By default, FastFloppy assigns them automatically but does not explain how. Perhaps based on FAT directory order? It is possible to use a config file to change to using image filenames that include the slot number.

The green LED is on all the time. I'm not sure if this is expected or it should flash during access. Not an issue at all.