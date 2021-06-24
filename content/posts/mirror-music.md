---
title: "Mirror Music"
date: 2021-06-23T20:15:04-04:00
draft: false
author: Mike Quigley
tags: ["max/msp", "jitter", "computer vision", "synthesis", "human computer interaction"]
---
{{< youtube hMbm4svvM6w >}}  

**Mirror music** is an interactive synthesizer that generates random pitches within a user specified key, triggered by motion, and characterized in range and timbre by color.   

Video input is split into a grid of 16 squares, or "pixels". Each pixel is analyzed for motion and color data. When motion in a pixel is detected, the color data at that moment is sent to 1 of 16 poly~ instances that generates a random pitch within the specified key, played for the specified length with attack, decay, sustain, and release.  

The level of green in the pixel is used to control the blend of sinewave and trianglewave generators. A predeominance of blue causes the randomly chosen pitch to be lowered an octave below the tonic, and a predominance of red causes the randomly chosen pitch to be raised an octave above the tonic.  

Controls for brightness, contrast, and saturation allow the user to manipulate the color data of each pixel before analyzation. Motion threshold allows the user to determine what level of motion triggers a note. With retrigger off, notes/pixels will not be retriggered until the entire notelength has passed. With retrigger on a pixel can continuously be triggered by generating new amplitude envelopes before the previous one finishes.  

This was my final project for Advanced Max/MSP/Jitter Programming with [Professor Dafna Naphtali](https://dafna.info/). The motion sensing and "visual feedback" was based off of a system similar to what was used in another project examining the effectiveness of [Visual Feedback In A Laptop Instrument](../visual-feedback-in-a-laptop-instrument).

Source code is available on [GitHub](https://github.com/mdquigley/mirror-music)

A look at some of the subpatches
![Mirror Music Subpatch 1](https://github.com/mdquigley/mirror-music/raw/master/screenshots/MirrorMusic02.png)
![Mirror Music Subpatch 2](https://github.com/mdquigley/mirror-music/raw/master/screenshots/MirrorMusic03.png)
![Mirror Music Subpatch 3](https://github.com/mdquigley/mirror-music/raw/master/screenshots/MirrorMusic04.png)
![Mirror Music Subpatch 4](https://github.com/mdquigley/mirror-music/raw/master/screenshots/MirrorMusic05.png)

