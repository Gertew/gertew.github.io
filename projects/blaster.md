---
layout: post
title: 'Blaster'
---

#### Role - Solo Developer
#### Engine/Platform - Unreal Engine 5 (Windows)
#### Practiced Skills - C++, Multiplayer Programming, Server/Client Architecture

Blaster is an Unreal Engine 5 project from an online course (UE5 Multiplayer Shooter) where I am learning how to create a fully online networked 3rd person multiplayer shooter. This is a solo project course. The project is implemented in C++ and uses Blueprints to easily modify variables on assets derived from their C++ class. The project features 3 game modes: Free For All, Team Deathmatch, and Capture The Flag. I was so happy when me and friend were able to playtest the game over the internet. For the majority of the course, I did a lot of local testing but when my friend was able to join as a remote play tester it was magically. Even then it still wasn't perfect, we were able to fix many bugs due to there not being enough latency between the server and clients.

{% include image.html url="https://drive.google.com/file/d/1f_QrxpTGNix3DJnH0_eyeiemKIsXvRTg/view?usp=sharing" image="projects/blaster/Blaster_Splash.png" %}

This project uses Steam to handle connecting players together. If Steam is not opened then the project defaults it to a LAN match. This project uses a custom MultiplayerSessionsSubsystem that manages a player's connection to a game session. This subsystem is a wrapper that handles the inner workings of the actual OnlineSubsystem in order provide more convience when used.

{% include image_no_url.html image="projects/blaster/Blaster_MultiplayerSessionsSubsystem.png" %}

Outside of this aspect, mostly everything else uses a lot of the built in Unreal Engine support for multiplayer. One aspect that I was able to implement was a form of Server Side Rewind in order to allow players with some latency the ability to get hits. As a very laggy player would never be able to hit someone who has a lower latency to the server and this makes the laggy player's experience suboptimal. This is the running around the corner and dying dilemma to which this project supports in some way. 

In order to provide a more fair playing field, the Server is saving transformations of the character's hitboxes every frame. When a player fires their weapon this is done locally and then a hit confirmation request is sent to the Server. Once the request reaches the Server, the Server will check the time of the supposed hit with the oldest hitbox data it has stored. If the hit is older than the oldest hitbox data then this client is deemed too laggy and will disregard their request. Otherwise the Client's request will be processed where the Server will find the closest hitbox data with respect to the time of the hit. If there is no exact hitbox data with the exact same time then the hitbox data will be an interpolated with data from before and after the hit time. From this point forward, the Server will do a line trace from the weapon in the direction it fired to determine for a hit for the case of a hit scan against the interpolated hitbox data. In the projectile case, it is very similar but instead of a line trace the Server instead try to predict the projectile's path from the weapon barrel and see if it hit anything along the way.

{% include video.html video="projects/blaster/Blaster_ServerSideRewind_FrameHistory.mp4" %}
{% include video.html video="projects/blaster/Blaster_ServerSideRewind_CodeOrganization.mp4" %}

In the event of an actual hit against a player, the player who fired the weapon is rewarded with the hit. This makes it feel like the player is not being punished for being a little laggy with the tradeoff being that sometimes a player might die when they are behind cover on their screen. In order to make this not favored towards laggy players, players with too high of latency will not be allowed to use Server Side Rewind thus keeping the game more balanced.

Even though I was able to create this project with help from the online course. It by no means that this project is perfect and bug free. There are a lot of optimizations to improve the network performance. As well as additional game features to diversify and balance the game more.