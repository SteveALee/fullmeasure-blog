---
title: ASIO channel routing on Windows
date: 2020-12-08 16:47:00
tags:
---

## My problem

The [McMillen K-Mix](https://www.keithmcmillen.com/products/k-mix/) is an awesome piece of Kit; an audio interface, mixer and MIDI control surface all in one. The first two features are proving to be perfect for various configurations of kit during my remote music lessons and practice. Now that I'm starting to record as I play along my required setup has become a more little complex.

The PC main audio output always appears on channels 1 & 2 when using the K-mix Windows sound driver.

But here's the thing. THe K-Mix uses channel 1 & 2 for the balanced mic and Hi Z instrument inputs. 

So that's a clash and I spent a significant amount of time exploring options for routing the PC non ASIO sound out to channels 3 & 4 (say) of the K-Mix ASIO driver.

Nada! There are solutions for routing ASIO from DAWs to non ASIO drivers and apps. There are also a few software "cables" that provide a connection between virtual outputs and inputs. But nothing to provide the simple non ASIO to ASIO device driver with channel routing.

## Huzzah

But, TADA, final I found VB Audio [Hi-Fi CABLE & ASIO Bridge](https://www.keithmcmillen.com/products/k-mix/).This nifty little driver and app combination alows routing from it's virtual output sound device through to a ASIO device and from the ASIO device input to the virtual input sound device. And all with channel mapping just as I required.

## Getting set up

It's really easy to setup.

![ASIO Bridge settings and sound output](/images/ASIOBridge.png)

* download and install the Hi-Fi CABLE & ASIO Bridge
* reboot
* click on the speaker icon in system tray and select `HiFi Cable input`
* run `ASIO Bridge`
* click the central button so it says `ASIO On`
* click on `Select A.S.I.O device` below the button and select your ASIO device (K-mix in my case)
* on the left, click on the 3 until it shows 1 and then the 4 until it shows 2
* repeat so 1 says 3 and 2 says 4
* adjust mixer levels so chanel 3 & 4 can be heard

That's it.

Note, you need to have the ASIO Bridge app running in order to hear any sound.

## Outcome

Now I can plug my bass into the mixer channel 1, set the eq etc. and jam along to iReal Pro (Android app playing in Bluestacks) or other things on YouTube. Recording in Audacity is also possible and here's my script solution for [building Audacity with ASIO support](../../../03/28/audacity-with-asio/). I can also play my flac files with Plex in Firefox.  

I recently upgraded my old Tannoy HiFi speakers and NAD Amp to Presonus Eris E8 XT Active Studio Monitors using balanced TRS cable direct from the K-Mix. The sound is fantastic.
