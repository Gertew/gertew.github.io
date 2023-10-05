---
layout: post
title: 'Tank Wars'
---

##### Role - Engineer (Team of 2)
##### Engine/Platform - Custom 2D Engine (Windows)
##### Practiced Skills - C#, Communication, Pair Programming

Tank Wars was a multiplayer game made in Spring of 2021 in the University of Utah's Software Practice I course (CS 3500). This project is a simple top down free for all multiplayer game where each player controls a tank and gets points by exploding other player tanks. This project was meant to teach students the Server Client model and where we as students implemented the Model View Controller (MVC) architecture in order to achieve this. This project was in teams of 2 and was implemented using C#. I was the main programmer that implemented most of the server sided logic where we abided by the Server Authoritative model. The Server is the one who controls the actual game world (Model). Clients (View) would be rendering the information that the Server would send to each client. Clients could request actions (Controller) to be done like shooting a projectile and moving. The Server was going to need to handle concurrency well as many players could join and there would be many race conditions. This was problem was solved by using mutex locks on critical sections of the game world.

I am most proud of the work me and my teammate put into our custom 2D engine that allows this simple game to even work. This was the first real big project that I ever did with another person. This was during COVID and there were so many things going on in each of our own lives. We managed to find a good amount of time each week to pair program on this project. The pair programming that we did helped us learn from each other so much more and faster than if we had done it separately. This project really connected a lot of dots for me relating to software and programming in general. I learned how a GUI works in the MVC model due to delegates. Networking and object oriented programming work really well together especially in the context of a game. The model for this project was to represent the game world due to it being comprised of many separate objects like: tanks, walls, and projectiles that all can exist within the world.


{% include image.html url="../downloads/TankWars.zip" image="projects/tankwars/TankWars_Splash.png" %}
