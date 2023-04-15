---
title: Using a Gotek USB Floppy Emulator in a Roland XP-50
date: 2023-03-15 16:47:00
tags:
---

Further to my last post on recommissioning my Roland XP-50 workstation, I replaced the floppy drive with a Gotek floppy emulator. I can now use USB memory sticks to store files and transfer them between my PC. For example MIDI .mid files can be put on a virtual floppy on a USB via my PC and then accessed by the XP-50.

The [Gotek USB Floppy Emulators](https://www.gotekemulator.com/) are well proven replacements for 3.5 inch floppy drives. They use ubiquitous USB memory sticks to hold a large number of virtual floppies using image files. Replacing the stock firmware with FastFloppy provides a reliable solution that works in almost all possible applications, including old Roland workstations like my XP-50.

Additionally, the WinImage program is useful for working with the images on a PC, including inserting and removing files.

It took me a while to figure out the correct and current info so I provide a summary here. It should also apply to other musical instruments with floppy drives.

![The Gotek in situ](/images/XP-50/gotek.jpg)
<figcaption>Gotek Floppy Emulator working in the XP-50</figcaption>

## Acquiring a Gotek

Gotek only supply in bulk but there many, many suppliers who will sell individual units to end users. Confusingly, there are many models available from Gotek and several variations have appeared over time. Many suppliers badge them and do not specify which version they provide. In addition, it is possible to upgrade the stock Gotek hardware with a better display and a rotary control. Some suppliers provide these updates pre installed.

Fortunately, this complexity is well covered in the [FastFloppy Gotek model documentation](https://github.com/keirf/flashfloppy/wiki/Gotek-Models).

I played safe and purchased a device specifically listed as [SFR1M44-U100K GoTek 1.44MB USB SSD Floppy Drive Emulator for YAMAHA KORG ROLAND Electronic Keyboard](https://www.gotekemulator.com/P_view.asp?pid=55) with 7 segment display and two buttons. However, I didn't know about the significant variation over time that FastFloppy details. Fortunately though, the model I received was a recent SFRKC30AT3 using the Artery AT32F415 chip (not the very latest). Gotek changed chip manufactures to Artery due to escalating costs.

## Setting up the Gotek

The first thing to do is Install FastFloppy. The Gotek rive is then installed replacing the floppy drive.

Installing FastFloppy on the Gotek is straightforward. Connect the Gotek to a PC with a USB cable, install the Artery driver and flashing software on the PC and use it to install FastFloppy to the Gotek. Full details are provided in the [FastFloppy Firmware Programming docs](https://github.com/keirf/flashfloppy/wiki/Firmware-Programming).

Swapping the drives is also straightforward, as you'd expect. In my case just 4 screws and after finding conflicting advice over jumper settings only a single S0 jumper was needed. The floppy drive was configured for S0 (first drive) and Ready signal on pin 24, but FastFloppy just required the S0 jumper. 

Turning on the XP-50 after installation, the Gotek display showed "F-F" (for FastFloppy) if all is well.

## Testing and Using the Gotek

After some confusion caused by various aged articles, I was pleased to discover the latest FastFloppy firmware makes it "easy peasy" to use the drive. Both in the XP-50 and transferring files between the XP-50 and a PC. I performed an initial "smoke test" as follows:

- Format the USB stick as FAT using DiskManager (or Windows Explorer)
- Grab the FastFloppy empty 1.44MB image file from the [image library](https://github.com/keirf/flashfloppy-images/blob/master/README.md) and put two copies on the stick
- Plug the stick into the Gotek and turn the XP-50 on
- After startup, the Gotext buttons allow switching between slot "000" and "001" indicating the two images are seen
- Use XP-50 Disk utilities to check the images can be seen format one - takes over a minute but then the Gotek is emulating a slow floppy
- Plug the stick into a PC running WinImage
- Grab a MIDI .mid file, test it on MuseScore and add it to the formatted image file (I used Bach's Air on a G String from MidiWorld)
- Pop the stick back in the XP-50, select the image, load the file, select General Midi mode and play it
- Enjoy the cheesy GM sounds

That's it. I ca read and write files to the floppy images on both devices.

## Unknowns

It's not clear if FastFloppy has any restrictions on the type or size of USB memory stick used. I used a 32GB SansDisk without problems. 

Access on the XP-50 is fairly slow but I guess FastFloppy is emulating timings to keep the host hardware happy. There are some notes on how to mitigate slow Roland access which I'll keep in mind in case I feel I needed it.

Unless I upgrade the Gotek hardware I will need to keep track of the slot numbers for each image file. By default, FastFloppy assigns slot numbers automatically but does not explain how. Perhaps based on FAT directory order? It is also possible to use a config file to change to using image filenames that include the slot number.

The green LED is on all the time. I'm not sure if this is expected or it should flash during access. Not an issue at all.