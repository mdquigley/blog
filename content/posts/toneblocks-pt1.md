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
Essentially, Toneblocks combines two frameworks, Tone.js and Blockly, in a proof of concept project that seeks to make programming less daunting to novice users (thus, a block-based interface) interested in creating music with code (thus, synthesis with web audio). 

[Tone.js](https://tonejs.github.io/) is a web audio JavaScript framework built for musicians and audio programmers looking to create interesting web-based audio applications.  Particularly of interest to me was Tone.js's synchronization abilities. Specifically, the Tone.js Transport is handy way to sync musical events in a more intuitive way than some other code-based audio environments. 

[Blockly](https://developers.google.com/blockly) is a framework for creating block-based graphical user interfaces, similar to those found in [Scratch](https://scratch.mit.edu/), a popular block-based programming language developed at MIT with a large and active user base. 

## Lets Make Something!

To get a sense of what's going on here, we're going to walk through three versions of a traditional "Hello World" program. First, we'll get Tone.js set up and play our first musical note. Second, we'll get Blockly set up and create our first block in a simple workspace. Finally, we'll get these two tools working together and create a programmable block that plays a note. 

### Hello Tone
Create a project folder and a file `index.html`. In our file, lets load in Tone.js with a `<script>` tag in the `<head>` section. This will allow us to utilize all that Tone.js has to offer
```
<!DOCTYPE html>
<html lang="en">
<head>
...
    <!-- NOTE: version & integrity hash may have changed since publishing -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.7.68/Tone.js"
        integrity="sha512-eBjUIAF/NN5kcGlFXz5n9UMAv+LKYFqtGnqHu83qQCXRJaL6iSsjFeOdft9AK9lD/9meTkj/5ctn6DGV5rz6Pg=="
        crossorigin="anonymous"></script>
```
and set the `<head>` info as you normally would.
- loading tone.js via script in header 
- create play button
- script tag in body to create synth and onclick function to play a note

### Hello Block

- basic injecting Blockly into the index.html file
- creating a block that prints Hello World

### A Tone Block
- creating a block that plays a note based on MIDI number input