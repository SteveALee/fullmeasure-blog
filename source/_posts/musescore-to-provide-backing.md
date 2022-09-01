---
title: Using MuseScore to provide backing tracks from scores
date: 2022-09-02 12:01:00
tags:
---

## Using MuseScore to provide backing tracks from scores for music practice

I'm studying for ABRSM Grade 1 on double bass which requires I learn 3 pieces as well as the usual scales etc. Each piece has piano accompaniment but no audio tracks are provided with the books containing the pieces and I don't have access to a plano player. It turns out [MuseScore](https://musescore.org/en) comes to the rescue with a really simple solution.

I was about to manually enter the dots into MuseScore when I wondered if I might be able to scan in. I expected the answer to be be "yes if you purchase expensive software" but my first web search threw up MuseScore own free service at https://musescore.com/user/login?destination=%2Fimport (you need a free account to access). This service is built on the [Audiveris](https://github.com/Audiveris) open source project and works much better than I expected.

To get working backing track you simply:

1. Scan the score to pdf - I used Epson Scan which handles multiple pages.
2. Upload to the MuseScore service and wait for the MuseScore file to become available for download.
3. Load the generated .mscz file into MuseScore and hit play.

In reality, you might need to tweak the file a bit for best results. For example, You might want to mute or remove the lead part if you'll be playing it. I had to change the voice and stop it transposing up an octave (bass is 8VA transposing). I'll also need to tweak the dynamics and articulations. But as you can see, it did an excellent job, including ignoeing my hand drawn arco notations.

<figure>
  <img
  src="/images/spag-score.png"
  alt="A scanned music score">
  <figcaption>Scanned Score</figcaption>
</figure>

<figure>
  <img
  src="/images/spag-musescore.png"
  alt="A screen shot of musecore notation">
  <figcaption>Converted Score file in MuseScore</figcaption>
</figure>
