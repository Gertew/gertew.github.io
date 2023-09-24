---
layout: post
title: 'Save The Animals'
---

Save the Animals was made in the Summer of 2023 in the University of Utah's Alternative Game Development course (EAE 3720). The game was made with the intent to try to bring awareness towards animal testing where you play as a lab tested rat that has gained super powers as a result. The team consisted of 9 team members. I worked on this game as the lead gameplay/UI programmer. I implemented the modular ability system and each individual ability, physics interactions, puzzle mechanics, main menu/pause/HUD UI, and enemy AI. This game was solely implemented using Unreal Engine's Blueprints due to the time constraint of the Summer semester in order to quickly iterate on the game mechanics.

{% include image.html url="http://www.gratisography.com" image="projects/savetheanimals/SaveTheAnimals_Splash.png" %}

Prototype
Superpowered Rat Abilities
In the original game pitch, the pitch mentioned that the rat character has been tested on and gained super powers but as a group we weren't sure what kinds of powers we wanted. Eventually through some ideation and voting we landed on powers that were more sci-fi and go help with the puzzle solving.

The first power we all agreed on that the rat should have was telekinesis and being able to manipulate objects with its mind and all other powers would be built with this concept in mind.

Telekinesis Implementation
{% include video.html video="projects/savetheanimals/STA_Telekinesis_Prototype.mp4" %}
The initial implementation used Unreal Engine's Physics Handler which works for the most part in terms being able to move a physics simulating object around but it did not behave in a way that we wanted. One reason being that moving it around using the Physics Handler means that its velocity is dictated by how fast you move the camera. In scenarios where the player would rapidly flip the camera and let go of the object it would send it flying which was a fun idea that we later revisited to properly support but we didn't like this.

In order to solve this, I needed to find a way to reduce the force from the Physics Handler. I looked into what variables and functions were available on the Physics Handler. I found out that there were multiple things that I could manipulate but the biggest one was "Interpolation Speed". This variable controlled how fast the Physics Handler would try to interpolate between where the object's current position was and to the position I was trying to move it towards. Just setting this to a slower number would just result in everything grabbed moving slower but we as a team felt that it made more sense that only bigger and heavier objects moved slower. Each actor in Unreal has a mass variable that you can leave to be automatically determined by the engine or you can manually set it. I could then use the object's mass alongside some constant I specify in order to divide the mass by the constant which I then set as the interpolation speed. This resulted in exactly what we wanted and is in the final build.

{% include image_no_url.html image="projects/savetheanimals/STA_Telekinesis_InterpolateSpeed.png" %}

