---
title: Roland XP-50 Cleanup
date: 2023-04-03 16:47:00
tags:
---

In the late 90's I had a few keyboard lessons with John Loose, centred around learning Billy Taylor's "I Wish I Knew How It Would Feel To Be Free". I brought a Roland XP-50 Music Workstation to play as my existing Yamaha PSS 680 had rubbish mini keys. It spent most of its subsequent life in the loft, though had a brief reprise when our daughter Keziah decided to learn Piano. I've just dusted it down to use for ear training pitch checking and a little tinkling with backing tracks for when I get back to bass. 

After 40 years the internal battery was on it's last legs, though seem to be just about working. Given the age there's the usual risk of leaking electrolytic capacitors warranting an inspection. Plus there's a few years of fluff to clear out. 

![XP-50](/images/XP-50/20230403_112125.jpg)

The Roland XP-50 musical workstation was released in the mid 90's, consisting of a JV-1080 sound module, a MRC Pro sequencer and a 61 key synth-style keyboard plus other controllers. I think I paid about £800 and they still appear on ebay for around £300 so was a good investment (as my tutor told me it would be). 

<blockquote>The XP-50 Music Workstation is a powerful new
instrument based on a 32-bit RISC processor with
8 Mb ROM waveforms. It is 64 note polyphonic, 16
part multitimbral, GM compatible, and contains 512
Preset Patches and 128 User Patches. Up to four
SR-JV80 Wave Expansion boards can be installed,
providing up to 40 Mb and 1,660 Patches. The
XP-50 also includes Reverb, Chorus, and 40 other
effects including Rotary, Distortion, Flanging,
Phasing, and MultiTap Delays. Its MRC-Pro
Sequencer features 16 Track linear and loop
recording, plus 100 Patterns, real time nondestructive editing, event editing, and Direct from
Disk song playback. The XP-50's Realtime Phrase
Sequencer (RPS) allows recorded Patterns to be
triggered from specific keys</blockquote>

The voices are Roland's sample + subtractive synthesis which produces some great sounds. They were top of thir game then with many sounds being designed by [Eric Persing](https://www.soundonsound.com/people/eric-persing-creating-spectrasonics). Some are definitely of the period having a strong techno feel. Expansion boards are available though I have none.

It uses Midi internally to connect the three components and also has comprehensive external MIDI support. Sadly, it uses 3.5 inch floppy disks for storage and I managed to throw out the demos discs with all my old redundant floppys and drives. Maybe MIDI sysex can be used or some sort of drive emulator might be available, Else saving and loading files is going to be difficult.

Decoding the XP-50's serial number it appears it was made in Dec. 97. It has CPU version 1.01 and ROM firmware 1.03 but I don't intend to look for firmware updates. Though it appears an update floppy might be available. 

The strip down was easy enough. Just 24 self tapping screws on the bottom to remove enabling it to be lifted off, giving access. I was greeted by a long overdue sight of a couple of TTL logic ICs, which brought back memories of breadboarding circuits.  I no longer have a can of compressed air for cleaning but it was not that bad and a good old fashioned blow and a brush shifted the worse muck. I decided not to strip the keys down as could clean them externally and didn't want to risk causing playing problems. The keyboard feels OK if not brilliant and is velocity sensitive with after touch.

I popped in a new 2032 battery, the kind used in PC motherboards, and thankfully could find no leaking caps. There is a slight 100Hz-ish noise from the power supply, almost certainly transformer core lamination buzz. It's only just noticeable though in a quiet room so not anything to fix .

After screwing the base back on a final wipe down with a brush and damp microfibre cloth made it look a good as new.

I had a slight panic when, on powering up, the screen remained blank though backlight. My hunch that the battery change might have caused the problem proved correct when a factory reset fixed it. Everything seems to work perfectly after a quick play and I ran most of the self test. Except, I have no floppies to use so will ask around to see if anyone has an unwanted stash.

Now to figure out how to record using the sequencer. Of course, I could just use a DAW with the MIDI but having an appliance is so much more satisfying then yet more work on a computer.

![ROM version screen](/images/XP-50/20230403_112227.jpg)
<figcaption>ROM version Screen</figcaption>

![Power supply](/images/XP-50/20230403_112915.jpg)
<figcaption>Power Supply</figcaption>

![Expansion sockets](/images/XP-50/20230403_112936.jpg)
<figcaption>Expansion sockets</figcaption>

![Main PCB](/images/XP-50/20230403_112945.jpg)
<figcaption>Main PCB</figcaption>

![Floppy drive and pitch bend controller](/images/XP-50/20230403_112949.jpg)
<figcaption>Floppy drive and pitch bend controller</figcaption>

![Keys](/images/XP-50/20230403_113002.jpg)
<figcaption>Keys</figcaption>

![Cleaned and squirted](/images/XP-50/20230403_121754.jpg)
<figcaption>Cleaned and squirted</figcaption>
