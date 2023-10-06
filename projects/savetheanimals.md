---
layout: post
title: 'Save The Animals'
---

##### Role - Lead Gameplay/UI Programmer (Team of 9)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - Blueprints, Agile Development, Interdisciplinary Teamwork

{% include youtube.html url="https://drive.google.com/file/d/1b0OMne9uc0og10-bX1XfIWRHfr1qkr4m/preview" %}

Save the Animals was made in the Summer of 2023 in the University of Utah's Alternative Game Development course (EAE 3720). The game was made with the intent to try to bring awareness towards animal testing where you play as a lab tested rat that has gained superpowers as a result. The team consisted of 9 team members. I worked on this game as the lead gameplay/UI programmer. I implemented the modular ability system and each individual ability, physics interactions, puzzle mechanics, main menu/pause/HUD UI, and some enemy AI. This game was solely implemented using Unreal Engine's Blueprints due to the time constraint of the Summer semester in order to quickly iterate on the game mechanics. My biggest accomplishment on this project was how I was able to polish the physics interactions within the game. The game ultimately became fun to play when this happened as enemies no longer seemed overwhelming and puzzle interactions were more intuitive.

{% include image.html url="http://www.gratisography.com" image="projects/savetheanimals/SaveTheAnimals_Splash.png" %}

#### Superpowered Rat Abilities
In the original game pitch, the pitch mentioned that the rat character has been tested on and gained superpowers but as a group we weren't sure what kinds of powers we wanted. Eventually through some ideation and voting we landed on powers that were more sci-fi and helped with the puzzle solving.

The first power we all agreed on that the rat should have was telekinesis and being able to manipulate objects with its mind and all other powers would be built with this concept in mind.

### Telekinesis Implementation
{% include video.html video="projects/savetheanimals/STA_Telekinesis_Prototype.mp4" %}
The initial implementation used Unreal Engine's Physics Handler which works for the most part in terms of being able to move a physics simulating object around but it did not behave in a way that we wanted. One reason being that moving it around using the Physics Handler means that its velocity is dictated by how fast you move the camera. In scenarios where the player would rapidly flip the camera and let go of the object it would send it flying which was a fun idea that we later revisited to properly support but we didn't like it functioning like this.

In order to solve this, I needed to find a way to reduce the force from the Physics Handler. I looked into what variables and functions were available on the Physics Handler. I found out that there were multiple things that I could manipulate but the biggest one was "Interpolation Speed". This variable controlled how fast the Physics Handler would try to interpolate between where the object's current position was and to the position I was trying to move it towards. Just setting this to a slower number would just result in everything grabbed moving slower but we as a team felt that it made more sense that only bigger and heavier objects move slower. Each actor in Unreal has a mass variable that you can leave to be automatically determined by the engine or you can manually set it. I could then use the object's mass alongside some constant I specify in order to divide the mass by the constant which I then set as the interpolation speed. This resulted in exactly what we wanted and is in the final build.

{% include image_no_url.html image="projects/savetheanimals/STA_Telekinesis_InterpolateSpeed.png" %}

### Other Abilities
I knew that we wanted multiple abilities and a way to easily switch between acquired abilities. I needed to implement the abilities in a way that allows for reuse of similar functionality and could be added dynamically. I ended up creating a Base Ability class that was an Actor Component that could added to the player during gameplay.

{% include image_no_url.html image="projects/savetheanimals/STA_Base_Ability.png" %}

The Base Ability class handled activation/deactivation of the ability, cooldown, and ability cost. Each child's ability would only need to concern itself only when it was activated and deactivated. Some abilities were simply just modifying attributes on the character like movement speed, force applied when walking, jump height, and time dilation. Their implementations were simple but one problem that we ran into was activating multiple abilities/modifying the same attributes at the same time. If one ability was modifying an attribute then it would be overridden when another one activates/deactivates.

This became a massive headache as we had multiple things all modifying the same value. In searching for a solution to this, I looked towards games with RPG attributes that I had played. One game was League of Legends, where in that game you can buy items, have buffs/debuffs applied to the character, and have abilities that use those attributes. This system is called Gameplay Ability System, where you can have stat modifications and cooldowns on abilities. On this project, I felt there was no need to fully use this system and only needed some parts of it. The stat values you see while playing League of Legends is a final calculated value where there were multiple modifiers all adjusting some base value. I looked into how that game calculated its final value where flat modifiers that add or subtract are added in the beginning and then multiplicative modifiers are then calculated on that.

{% include image_no_url.html image="projects/savetheanimals/STA_Attribute_Calculation.png" %}

By having to calculate the final value, each ability would need to specify what type of modifiers it was applying. As well as how much it was modifying each of them by. This proved to be fine if the modifier values stayed constant but one designer on my team proposed the idea of having the abilities gain more power with how mutated you became throughout the game. While I was researching about modifying attributes, I looked back at my experience with playing League of Legends where you can level your abilities with skill points and it shows you in game what the values are of your ability per skill level. I realized that this data must be held in some kind of table that could be easily modified as in the case of League of Legends. I had previous experience with working with a DataTable in Goom but with a DataTable you have to specify the columns ahead of time and we were planning to allow the player to be as mutated as they wanted. What I really wanted was basically a curve where I can specify points on a 2D grid where mutation levels are the x coordinates and modifier values are the y coordinates.

{% include image_no_url.html image="projects/savetheanimals/STA_Ability_CurveTable.png" %}

#### Telekinesis + Breakable Objects
{% include video.html video="projects/savetheanimals/STA_Breakable_Physics_Objects.mp4" %}
The game was a stealth game and we wanted ways to distract the scientists. The primary ability we wanted the player to always be using was Telekinesis. Using the Physics Handler grabbing an object simulating physics was no problem but grabbing a fractured mesh that was created using Unreal's Chaos Destruction not so easy. Due to limitations with how the Physics Handler manipulates objects by directly interacting with a root physics bone, a GeometryCollection did not have this. This was a really challenging problem. There are many ways I considered to solve this problem. 

{% include image_no_url.html image="projects/savetheanimals/STA_Breakable_Base.png" %}

One being was that I could use the GeometryCollection as a way to fracture and separate a mesh into pieces and then recombine with multiple components inside a Blueprint and have all the pieces parented to something that simulated physics. Another way was to manually determine whether or not an impact would break the object and spawn the broken version. I went with the later approach as I didn't want to manually be adding potentially dozens or more of static mesh components into a single blueprint as designers on my team would most certainly miss one. By manually breaking the objects, I could also keep some of the logic separated where the unbroken Blueprint handles checking for a break event and then spawning GeometryCollection before applying a force field to break it.

{% include image_no_url.html image="projects/savetheanimals/STA_Physics_Object.png" %}

#### Scientist Captured Sequence
{% include video.html video="projects/savetheanimals/STA_Captured_Initial.mp4" %}
{% include video.html video="projects/savetheanimals/STA_Captured_Working.mp4" %}
Another idea designers on the project came up with was that animals squirm and what if our player could squirm when captured allowing for a chance to escape. I took what they had described and put a Box Collision on the Scientist's hand and would activate its collision when the grab animation was played. From there I could attach whatever they grabbed to their hand. If it was the player then I could start a timer to countdown. If the player doesn't escape before the timer stops then they are to be captured and put back in their cage.

It worked initially but there wasn't any real sense of danger and accomplishment when being grabbed as you could just mash and escape every time. In order to make the capture sequence more dramatic and tense, I created a Level Sequence that was set around the Scientist and animated the camera getting closer to the scientist. I created a UMG Widget that would display a circle each time you press the struggle button and with each press it would also shake the screen. My team really liked what I had done with the presentation of the sequence but still felt like even with this you could always mash the struggle button. I solved this by keeping track of how many times the player had been captured. Due to my prior research into CurveTables from the abilities, I applied the same thinking here where the number of captures will evaluate the curve and get the corresponding required amount of struggle attempts. This was easily adjustable by designers to their liking.
{% include video.html video="projects/savetheanimals/STA_Captured_Polished_Capture.mp4" %}
{% include video.html video="projects/savetheanimals/STA_Captured_Polished_Free.mp4" %}
