---
title: Relative Notes Musical Companion App
date: 2024-04-25 12:01:00
tags:
---

If your musical life is largely score driven then there's a fair chance you've not had much experience with or interest in relative pitch, So you may not have learnt how much it can benefit your musicality. For example, easier learning of pieces or finding where you are when getting lost following a score. Musical scores use absolute pitch with each note / pitch being specified and played as appropriate by learning dot to finger mapping.

Alternatively, musicians who tend to play by ear or work with other musicians in variable adhoc groups, like in jam or recording sessions, need a flexible approach to musical pitch. After all, they may be called on to play a tune in any key at the drop of a hat. Systems such as Nashville numbering, relative Do Solfege and Roman numerals provide the necessary framework to support learning and playing melodies in a more flexible way.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4kNCYW4tiGQ?si=zVuWIzZrZl2W5lUL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

In fact, many musicians do dabble with Solfege as when young, but it usually gets forgotten as working with scores takes precedence in later musical education and life. That's a shame because Solfege is a powerful tool for both learning and performance.

There are fortunately a few musical organisations that promote relative pitch. [Musical U](https://www.musical-u.com/), [Improvise for Real](https://improviseforreal.com/) and [Danny Ziemann](https://www.dannyziemann.com/) for example all use it as a core part of their training and practice.

One thing that soon become apparent though is the lack of tools that support relative pitch. There are ear training apps that use intervals or solfege. In addition, MuseScore supports putting Solfege syllables in note heads, an interesting addition. As an aside, my first ever foray into contributing to open source projects was with GNU Solfege, a little Linux app designed to help learn Solfege.

Recently I spotted the lack of ability to import a chord sheet from iReal Pro into a scoring tool and get the chords changed to Roman numerals for relative notation. With MuseScore the labels are just text and there is no musical knowledge that could be used to swap the display. Fundamentally, whilst scores have a key signature, these are ambiguous in defining the key, For example, a key signature with no accidentals can indicate C major or A minor, or even one of the other 5 modal keys.

When I'm practicing solfege exercises I sometimes sing or play the correct pitch yet use the wrong syllable. I wanted something to help me recognise that situation when it occurs. One solution is recording oneself as mistakes can often be spotted when listening back. But I wanted to try something a little more immediate, so I created a new musical companion app:

[**Relative Notes**](https://relative.musicpracticetools.net/)

The concept is simple: monitor what is being played and display the syllable or number that represents the played note relative to a reference tonic note (key centre). Ideally it will work with singing as this is a fundamental skill in developing relative musicality on any instrument.

When it came to researching components that I could use I found that whilst open source pitch recognition libraries do exist they really are not that effective in a range of applications. However, the recently released commercial pitch-to-MIDI program [Dubler](https://vochlea.com/) has proven itself to be excellent since I bought it on a whim. Furthermore, [MIDI](https://en.wikipedia.org/wiki/MIDI) enabling the app makes it much more flexible.

The required features are few:

- Monitor MIDI traffic from a MIDI device, either external hardware or virtual software
- Display the syllable or number
- Alow the tonic to be set, including to the current note

Technically I used the excellent [SvelteKit](https://kit.svelte.dev/) JavaScript web framework in Static Site Generator (SSG) mode and hosted on [Netlify](https://www.netlify.com/). I've had it working on Windows (Edge and Firefox) and Android (Chrome not Firefox). If Safari supports the Web MIDI spec it should work on Apple platforms too.

Do give it a go and see what you think. Is it at all useful? What other tools would be useful for using Relative Pitch?
