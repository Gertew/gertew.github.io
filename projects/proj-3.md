---
layout: post
title: 'Tank Wars'
---

Tank Wars was a multiplayer game made in Spring of 2021 in the University of Utah's Software Practice I course (CS 3500). This project is a simple top down free for all multiplayer game where each player controls a tank and gets points by exploding other player tanks. This project was meant to teach students the Server Client model and where we as students implemented the Model View Controller (MVC) architecture in order to achieve this. This project was in teams of 2. I was the main programmer that implemented most of the server sided logic where we abided by the Server Authoritative model. The Server is the one who controls the actual game world (Model). Clients (View) would be rendering the information that the Server would send to each client. Clients could request actions (Controller) to be done like shooting a projectile and moving. The Server was going to need to handle concurrency well as many players could join and there would be many race conditions. This was problem was solved by using mutex locks on critical sections of the game world.

{% include image.html url="downloads/TankWars.zip" image="projects/proj-3/TankWars_Splash.png" %}
