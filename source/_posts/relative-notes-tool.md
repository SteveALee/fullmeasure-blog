---
title: Relative Notes Musical Companion App
date: 2024-04-25 12:01:00
tags:
---

If your musical life is largely score driven then there's a fair chance you've not have much experience or interest with relative pitch, Nor will you have learnt how much it can benefit your musicality. For example, improved learning or finding where you are when playing a score and getting lost. Musical scores encourage absolute pitch activities with each note / pitch being specified in the score and played as appropriate by learning dot to finger mapping.

Alternatively, musicians who lean more towards playing by ear or working with other musicians in variable adhoc groups like at jam or recording sessions, have to have a flexible approach to musical pitch. After all, they may be called on to play a tune in any key at the drop of a hat. Systems such as Nashville numbering, relative Do Solfege and Roman numerals provide the necessary frameworks to support learning and playing melodies in this way.

In fact many musicians do dabble with Solfege as children, but it usually gets forgotten as working with scores takes precedence in later musical life. That's a shame because Solfege is a powerful tool for both learning and performance.

There are fortunately a few musical organisations that promote relative pitch for learning and performing. [Musical U](https://www.musical-u.com/), [Improvise for Real](https://improviseforreal.com/) and [Danny Ziemann](https://www.dannyziemann.com/) for example use it as a central part of their courses.

One thing that soon become apparent though is the lack of tools that support Relative Pitch. There are a few ear training apps that use intervals or solfege. In addition, MuseScore supports putting Solfege syllables in note heads on its scores, an interesting relative pitch addition to absolute tooling. As an aside, my first ever foray into contributing to an open source project was with GNU Solfege, a little Linux app designed to help learn Solfege.

One recent gap I spotted was the lack of ability to read a chord sheet (exported from iReal Pro) into a scoring tool and get the chords changed to Roman relative so they work in any key. The issue with MuseScore which I tried is the labels are just text and there is no musical knowledge that could be used. In fact, whilst scores have a key signature, they are ambiguous in defining the key, For example, a empty key signature with no accidentals can indicate C major or A minor, or one of the other 5 modal keys.

One issue I find with my own solfege exercises is I tend to sing or play the right pitch fairly reliably but sometimes have the wrong syllable in mind. I wanted something to help me recognise that situation as it happens. Recording oneself is a great tool for this as mistakes can often be spotted when listening back. But I wanted something a little more immediate. So I built my latest little musical companion app:

[**Relative Notes**](https://relative.musicpracticetools.net/)

The concept is simple: monitor what is being played and display the syllable or number that represents the played note relative to a reference tonic note (key centre). Ideally it will work with singing as this is a fundamental skill in developing relative musicality on any instrument.

When it came to researching components that I could use I found that whilst open source pitch recognition libraries do exist they really are not that effective in a range of applications. However, the recently released commercial pitch-to-MIDI program [Dubler](https://vochlea.com/) has proven itself to be excellent since I bought it on a whim. Furthermore, making my little app [MIDI](https://en.wikipedia.org/wiki/MIDI) enabled makes it much more flexible than working with audio.

So the features are few:

- Monitor MIDI traffic from a MIDI device, either external hardware or virtual software
- Display the Syllable or number
- Alow the tonic to be set, including to the current note

For the more geeky reader I used the excellent [SvelteKit](https://kit.svelte.dev/) JavaScript web framework in Static Site Generator (SSG) mode and hosted on [Netlify](https://www.netlify.com/).

Here's a short video demonstrating how it works:

Do give it a go and see what you think. Is it at all useful? What other tools would be useful for using Relative Pitch?
