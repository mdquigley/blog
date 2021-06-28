---
title: "Toneblocks Pt1"
date: 2021-06-28T12:43:36-04:00
draft: true
author: Mike Quigley
tags: ["thesis", "web-audio", "tonejs", "blockly", "music-technology"]
---
![](../../images/toneblocks.gif)

## Overview
Toneblocks is a web-based musical creative coding environment, built with [Tone.js](https://tonejs.github.io/) and [Blockly](https://developers.google.com/blockly). Originally developed as my thesis project for the Master of Music in Music Technology requirements at NYU Steinhardt. It is the basis for a paper/poster/video presentated at [NIME 2021](http://nime2021.org/program/#/paper/122) titled *Toneblocks: block-based musical programming*, co-authored with [Willie Payne](https://williepayne.com/). Watch a brief video demonstration on [Youtube](https://www.youtube.com/watch?v=rq1xpfseygU), or play around with it [on my site](https://quig.info/toneblocks).

This is **Part 1** of several posts taking a closer look at the application and its underlying mechanics.

## Tone.js & Blockly
Toneblocks is a proof of concept project that seeks to make programming less daunting to novice users (thus, a block-based interface) interested in creating music with code (thus, synthesis with web audio). A more detailed dive into the how and why of these considerations is available in the [paper](http://nime2021.org/program/#/paper/122) referenced above. But essentially, Toneblocks combines Tone.js and Blockly.  

[Tone.js](https://tonejs.github.io/) is a web audio JavaScript framework built for musicians and audio programmers looking to create interesting web-based audio applications.  Particularly of interest to me was Tone.js's synchronization abilities. Specifically, the Tone.js Transport is handy way to sync musical events in a more intuitive way than some other code-based audio environments. 

[Blockly](https://developers.google.com/blockly) is a framework for creating block-based graphical user interfaces, similar to those found in [Scratch](https://scratch.mit.edu/), a popular block-based programming language developed at MIT with a large and active user base. 

