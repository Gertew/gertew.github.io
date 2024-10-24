---
layout: post
title: 'Project Eon'
---

##### Role - Gameplay Programmer (Team of 3)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - C++, Procedural Generation, Rapid Prototype

{% include youtube.html url="https://drive.google.com/file/d/1PJIV-VF8WV4iy8LdJx8csiQeP1GuRdKH/preview" %}

Project Eon is a Character Action Dungeon Crawler where the player picks a hero from a cast of characters ranging across time in order to save the universe. This project is inspired by games like League of Legends, Super Smash Bros, Hades, and Enter the Gungeon where it has unique Heroes, gameplay altering items, and a procedurally generated dungeon.

The project currently uses placeholder Assets found on Unreal Engine marketplace specifically the Paragon character assets. This is a prototype to test and ensure game mechanics are functional. The project is by no means finished and we are still working on it.

I implemented the gameplay mechanics demonstrated by the character in the video above. For example the sword attack is sweeping a sphere continously as the sword moves to detect hits against enemies. The sword slashing attack combines sweeping and physics where I add a large force to the character launching them in the direction they are trying to attack while continously sweeping a sphere around the character.

{% include image_no_url.html image="projects/projecteon/ProjectEon_Dungeon_Generation.png" %}

I worked on implementing a Procedural Dungeon Generator that generates a grid of Rooms that allows the Player to navigate through the Dungeon. First focused my efforts on making sure that the generator was generating correctly before incorporating it with other game systems. The Dungeon is connected and there is a path from 1 room to another which makes it an undirected graph. First, I would pick a random starting room and from there pick a random valid direction until all rooms were generated. If the selected direction would lead to a dead end then I would pick a random room from the generated list that was not completed where being completed means it is surrounded by a valid room or edge of the grid. At the end, I made use of breadth first search algorithm to ensure that the graph is connected. In the picture above you can see the Dungeon represented as a grid of characters in the top left and the UI part which is a grid of interactable buttons is in the center.
