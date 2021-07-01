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
The [Tone.js API documentation](https://tonejs.github.io/docs/14.7.77/index.html) has lots of detailed information on what's available, as well as some example projects and real world demos/applications. To get us started, we'l try implementing a simple Synth object that that plays a note when a button is pressed.

Create a project folder and a file `index.html`.  
In our file, lets load in Tone.js from CDNJS with a `<script>` tag in the `<head>` section. Note the version and integrity hash may be different now than at the time this post was written.
```
<!DOCTYPE html>
<html lang="en">
<head>
...
    <!-- Load Tone.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.26/Tone.js"
    integrity="sha512-+esjJ8NSEfoB5Sr8R7jTcYxCR1Bd6q9C+WQC0JA2UXVPT8Mlo/TJqqyp0qeeoxFzkAaa8t6tZCHLGmw3oNI2Qg=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer">
    </script>
</head>
...
```
Now that Tone.js is loaded into our main file, we'll be able to use it to create a synth. Lets do that in a new `<script>` tag in the `<body>` section.
```
<!DOCTYPE html>
<html lang="en">
<head>...</head>
<body>
    <script>
        // Create synth and route output to destination
        const synth = new Tone.Synth().toDestination();
    </script>
</body>
...
```
In the code above, we create a new variable of type const, assign it a new [`Tone.Synth()`](https://tonejs.github.io/docs/14.7.77/Synth) object, and connect the the synth's output to the system's main output (likely your speakers) using the [`toDestination()`](https://tonejs.github.io/docs/14.7.77/Synth#toDestination) method.

We want the synth to play a note when we press a button. So lets create a button in the `<body>` section of the file. Then we can access the button in our `<script>` section and give it an *on click* function to trigger the synth.  
Checking the Tone.js docs, Synth objects have a [`triggerAttackRelease()`](https://tonejs.github.io/docs/14.7.77/Synth#triggerAttackRelease) method which takes a pitch value and a duration value. We'll use 'C4' for our pitch value, which is middle C on a piano, or ~261.63 Hz. And we'll use '8n' as our duration, so the tone will play for the length of an 8th note. 
```
<!DOCTYPE html>
<html lang="en">
<head>...</head>
<body>
    <!-- Create button element -->
    <button id="play">Play</button>

    <script>
        // Create synth and route output to destination
        const synth = new Tone.Synth().toDestination();

        // Assign button variable to button element
        const button = document.getElementById('play');

        // Add "on click" function to play note when button is pressed
        button.addEventListener('click', () => {
            synth.triggerAttackRelease('C4', '8n');
        });
    </script>
</body>
...
```
So now if you run your `index.html` file in your browser, you should see a single button with "Play" displayed. And when you press it, you should hear an 8th note of Middle C. Hello Tone! 🎵

### Hello Block
Next, we're going to implement a Blockly workspace and print "Hello Block" when the workspace code is run. The [Blockly API documentation](https://developers.google.com/blockly/) has a breakdown of its many features, as well as examples and tools to help with creating your own environment.  
First we'll load Blockly with a few `<script>` tags just like we did for Tone.js.

```
<!DOCTYPE html>
<html lang="en">
<head>
...
    <!-- Load Tone.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.26/Tone.js"
    integrity="sha512-+esjJ8NSEfoB5Sr8R7jTcYxCR1Bd6q9C+WQC0JA2UXVPT8Mlo/TJqqyp0qeeoxFzkAaa8t6tZCHLGmw3oNI2Qg=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer">
    </script>
    
    <!-- Load Blockly -->
        <script src="https://unpkg.com/blockly/blockly_compressed.js"></script>
        <script src="https://unpkg.com/blockly/blocks_compressed.js"></script>
        <script src="https://unpkg.com/blockly/javascript_compressed.js"></script>
        <script src="https://unpkg.com/blockly/msg/en.js"></script>
</head>
...
```
In its barest form, Blockly requires a *Workspace* on your page within which you can click and drag blocks from a *Toolbox*, a list of avilable blocks. To get this set up, we need to create a few HTML elements in our `<body>` section so that Blockly can "inject" its workspace. The example blocks we'll use in this *Toolbox* are taken from the [Blockly docs](https://developers.google.com/blockly/guides/configure/web/fixed-size).
```
<!DOCTYPE html>
<html lang="en">
<head>...</head>
<body>
    <!-- Create button element -->
    <button id="play">Play</button>

    <!-- Create Blockly elements -->
    <div id="blocklyDiv" style="height: 480px; width: 600px;"></div>
    <xml id="toolbox" style="display: none">
        <block type="controls_if"></block>
        <block type="controls_repeat_ext"></block>
        <block type="logic_compare"></block>
        <block type="math_number"></block>
        <block type="math_arithmetic"></block>
        <block type="text"></block>
        <block type="text_print"></block>
      </xml>

    <script>...</script>
</body>
...
```
Now that the elements are there, we need to "inject" 



- basic injecting Blockly into the index.html file
- creating a block that prints Hello World

### A Tone Block
- creating a block that plays a note based on MIDI number input