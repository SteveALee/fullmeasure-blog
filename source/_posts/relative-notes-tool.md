---
title: Relative Notes Musical Companion App
date: 2024-04-25 12:01:00
tags:
---

**The Relative Notes concept is simple: monitor what notes are being played and display the syllable or number that represents the played note relative to a reference tonic note (key centre). Ideally it will work with singing as this is a fundamental skill in developing relative musicality on any instrument.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/4kNCYW4tiGQ?si=zVuWIzZrZl2W5lUL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

If your musical life is largely score driven then there's a fair chance you've not had much experience with or interest in relative pitch, So you may not have learnt how much it can benefit your musicality. For example, easier learning of pieces or finding where you are when getting lost following a score. Musical scores use absolute pitch with each note / pitch played as appropriate by learning dot to instrument mapping.

Alternatively, musicians who tend to play by ear or work with other musicians in variable adhoc groups, like in jam or recording sessions, need a flexible approach to musical pitch. After all, they may be called on to play a tune in any key at the drop of a hat. Systems such as [Nashville numbering](https://en.wikipedia.org/wiki/Nashville_Number_System), relative Do [solfège](https://en.wikipedia.org/wiki/Solf%C3%A8ge), [Roman numerals](https://en.wikipedia.org/wiki/Roman_numeral_analysis) and the older [Figured bass](https://en.wikipedia.org/wiki/Figured_bass) provide the necessary frameworks to support learning and playing melodies or harmony in a more flexible way.

In fact, many musicians do dabble with solfège when young, but it usually gets forgotten as working with scores takes precedence in later musical education and life. That's a shame because solfège is a powerful tool for both learning and performance.

There are a few musical organisations that actively promote relative pitch. [Musical U](https://www.musical-u.com/), [Improvise for Real](https://improviseforreal.com/) and [Danny Ziemann](https://www.dannyziemann.com/) for example all use it as a core part of their training and practice.

One thing that soon become apparent though is the lack of tools that support relative pitch. There are ear training apps and games that help develop interval or solfège skills. In addition, [MuseScore](https://musescore.org/en/download) supports displaying solfège syllables in note heads, an interesting addition though not so effective in use. As an notable aside, my first ever foray into contributing to open source projects was with [GNU solfege](https://www.gnu.org/software/solfege/), a little Linux app designed to help learn solfège.

Recently I spotted the lack of ability to import a chord chart from [iReal Pro](https://www.irealpro.com/) into a scoring tool and get the chords changed to Roman numerals relative notation. With MuseScore the labels are just text and there is no musical "knowledge" that could be used to swap the displayed text. More fundamentally, whilst scores have a key signature, these are ambiguous in defining the specific key. For example, a key signature with no accidentals can indicate C major or A minor, or even one of the other 5 modal keys.

It's common When practicing solfège exercises to sing or play the correct pitch yet use the wrong syllable. I wanted something to help me recognise that situation when it occurs. A good solution is to record oneself as mistakes can often be spotted when listening back. But I wanted to try something a little more immediate (and geeky), so I created a new musical companion app.

Here it is: [**Relative Notes App**](https://relative.musicpracticetools.net/)

(Sorry Safari Users but it still does not support WebMIDI so you'll need to us another browser)

When it came to researching software components that I could use I found that whilst open source JavaScript pitch recognition libraries do exist they really are not that effective in a range of applications. However, the recently released commercial pitch-to-MIDI program [Dubler](https://vochlea.com/) has proven itself to be excellent since I bought it on a whim. Furthermore, [MIDI](https://en.wikipedia.org/wiki/MIDI) enabling the app makes it more flexible (unless you use Safari).

The feature requirements are few:

- Web based - naturally
- Monitor MIDI traffic from a MIDI device, either external hardware or virtual software
- Display the syllable or number
- Alow the tonic to be set, including to the current note

Technically I used the excellent [SvelteKit](https://kit.svelte.dev/) JavaScript web framework in Static Site Generator (SSG) mode in conjunction with [WebMIDI](https://webmidijs.org/) and [tonal](https://tonaljs.github.io/tonal/docs/). Hosting is on the wonderful [Netlify](https://www.netlify.com/). I've had the app working on Windows (Edge and Firefox) and Android (Chrome not Firefox). As mentioned earlier Safari users are out of luck but as the dev team have recently become better at supporting web standards there is some hope.

Do [give it a go](https://relative.musicpracticetools.net/)and see what you think. Is it at all useful? Can it [be improved](https://github.com/music-practice-tools/relative-notes/issues), while keeping it simple. What other tools could support for using Relative Pitch?
