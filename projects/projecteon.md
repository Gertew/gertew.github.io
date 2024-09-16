---
layout: post
title: 'Project Eon'
---

##### Role - Gameplay Programmer (Team of 3)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - C++, Procedural Generation, Leadership, Rapid Prototype

{% include youtube.html url="https://drive.google.com/file/d/15MMTjCtxmPDkTBxgtFa_-x-Usw_lT-6Z/preview" %}

Project Eon is a Character Action Dungeon Crawler where the player picks a hero from a cast of characters ranging across time in order to save the universe. This project is inspired by games like League of Legends, Super Smash Bros, Hades, and Enter the Gungeon where it has unique Heroes, gameplay altering items, and a procedurally generated dungeon.

{% include image.html url="https://drive.google.com/file/d/1f_QrxpTGNix3DJnH0_eyeiemKIsXvRTg/view?usp=sharing" image="projects/ivorfall/Ivorfall_Splash.png" text="Demo"%}

The project currently uses placeholder Assets found on Unreal Engine marketplace specifically the Paragon character assets. This is a prototype to test and ensure game mechanics are functional. The project is by no means finished and we are still working on it.

I implemented the gameplay mechanics demonstrated by the character in the video above. For example the sword attack is sweeping a sphere continously as the sword moves to detect hits against enemies. The sword slashing attack combines sweeping and physics where I add a large force to the character launching them in the direction they are trying to attack while continously sweeping a sphere around the character.

{% include image_no_url.html image="projects/ivorfall/Ivorfall_Mod_Details.png" %}

I worked on implementing a Procedural Dungeon Generator that generates a grid of Rooms that allows the Player to navigate through the Dungeon. First focused my efforts on making sure that the generator was generating correctly before incorporating it with other game systems. The Dungeon is connected and there is a path from 1 room to another essentially an undirected graph. I was able to make use of breadth first search algorithm to see if the graph is not connected. If it wasn't then I would attempt to add another room and repeat until the graph was connected. In the picture above you can see the Dungeon represented as a grid of characters in the top left and the UI part which is a grid of interactable buttons is in the center.
