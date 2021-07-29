---
title: "Get To The Gig"
date: 2021-07-28T16:04:49-04:00
draft: false
author: Mike Quigley
tags: ["games", "game audio", "game design", "development", "phaser", "tonejs", "pixel art", "javascript"]
---

![](../../images/gettothegig.png)

Get To The Gig is a simple browser based game written in JavaScript, built with the [Phaser 3](https://phaser.io/) and [Tone.js](https://tonejs.github.io/) frameworks.  

Check out the *work-in-progress* [demo](https://quig.info/gettothegig)  
Source code on [GitHub](https://github.com/mdquigley/gettothegig)

## About
I wanted to build a game that utilized music in an interactive way, and in which the music itself was composed via code as oppposed to only playing back audio files. I thought of a quest style game in which the player picks up instruments and a corresponding musical layer is added to the soundtrack.

## The Game
*"Load-in was 30 minutes ago and your bandmates are nowhere to be found. Find all your gear and get to the gig!"*  

The player is presented with a top-down map, with restricted tiles (water) and obstacles (trees & rocks). They must reach the music venue, but can only enter after they've collected all of their gear items. The player navigates with **W A S D** keys. As instruments are collected, a new music layer is added to the soundtrack. Other items (merch) play a sound effect but do not add musical layers. 

## Status
At present, the game is only a simple, single level. My main goals were to:
- build a game with Phaser 3
- utilize Tone.js to compose the game music
- have game mechanics interact with the music
- get some experience using the [Tiled](https://www.mapeditor.org/) map editor
- make some pixel art using [Pixilart](https://www.pixilart.com/)
    - I'm pretty stoked on the venue graphic, based on thee beloved [Shea Stadium BK](http://liveatsheastadium.com/)
    ![](https://raw.githubusercontent.com/mdquigley/gettothegig/master/src/assets/sprites/shea-3d-128.png)

Some possible future development may include:
- additional levels (with other venues near and dear to my heart)
- additional songs for each level
- more complexity in the instrumental parts
- more complexity in song structure (ie. verse, chorus, bridge for longer levels)
- songs with varying styles to match level vibes
- tone.js transcriptions (covers) of friends' songs (with permission)
- custom sprites for player and items
- Stakes!
    - a countdown to gig time
    - the tempo increases as time left decreases?
    - scores based on time
    - NPCs that interfere with the quest
    - Lasers? magic bolts? to counter NPCs

## Deployment
I wrote a short tutorial on [deploying the game with Webpack and GitHub pages](../deploying-a-game-with-webpack-and-github-pages).

## Screenshots
### Title Scene
![](../../images/gettothegig-title.png)

### Level01
![](../../images/gettothegig-play.gif)