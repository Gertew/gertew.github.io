---
layout: post
title: 'Goom'
---

#### Role - Lead Gameplay Programmer (Team of 7)
#### Engine/Platform - Unreal Engine 5 (Windows)
#### Practiced Skills - Blueprints, Interdisciplinary Teamwork, Saving Games

{% include youtube.html url="https://www.youtube.com/embed/U32NmC4CM8k" %}

Goom was made in the Spring of 2023 in the University of Utah's Traditional Game Development course (EAE 3710). This game is a Isometric slime game that drew inspiration from games like: Tunic, Death's Door, and The Legend of Zelda: The Wind Waker. There were 7 team members and 2 addition outside helpers. I worked on this game as the lead programmer for the gameplay aspects. On this game, things I implemented were: the projectile shooting mechanic, elemental interactions, player ability system, logic puzzle states, enemy combat, and blending between cinematics and gameplay. All of these were implemented using Blueprints due to the majority of the team being new to Unreal Engine at the time.

{% include image.html url="https://ninjakgames.itch.io/goom" image="projects/goom/Goom_Splash.png" %}

#### Elemental Slime Abilties
I was most proud of this system that I was able to build that not only interacted with combat but also with puzzles. A big part of it was to due the designers on my team laying the foundation and allowing for me to build on top of it. Each element has a combat and puzzle solving purpose. This duality allowed for designers on the team to flex their puzzle brain.
In the original pitch, the pitch was titled Slime Sunlight. During the pitching process, the vision holder was asked a question about how sunlight/light interacted with the main slime character. The vision holder said they thought it would be cool if different colors of light would affect of the slime played. Once I had joined the team, I thought this was a cool mechanic and was challenging at the time due to my inexperience with making games.

How I initially approached the abilities was that I used an Enum to list all possible elemental states the slime could be in. In final game we ended up with Fire, Water, Electric, and Slime where Slime would be the base/default state while all of the other ones would be acquired through exploration and puzzles. Once I had defined the Elemental State Enum, everything else was to switch on this state and define behavior/logic that dealt with each case. Designers on the project wanted each and every element to feel unique and powerful. I also worked closely with the VFX artist and UI designer to help implement their visuals with the corresponding gameplay elements.

### Slime Element
{% include video.html video="projects/goom/Goom_Slime_Prototype.mp4" %}
{% include video.html video="projects/goom/Goom_Slime_Final.mp4" %}
The designer who designed the Slime element wanted it to be easy and straight forward to use as this would the first element the player uses. The designer mentioned that when they first try a game they tend to hold buttons down in the event something would happen or push every button. Since our game had limited controls we believed that only the former would be common case. They had proposed to make our slime character shoot charge projectiles. At first I didn't know how I could accumulate time into a variable. Upon doing research, I was able to find out that I could use DeltaTime in order to add to a running total the amount of time that has passed and check if the running total had reached a charge threshold. This seemed doable but I asked my professor at the time and he recommended that I take a look at Timers and Timelines. This seemed to be exactly what I was looking for. I was able to use a Timeline where I set the time it takes until max charge and then place a threshold for how long it needed to be held before you could fire.

With the primary attack finished, I felt this was doable for the other elements and it would be very similar. Designers on my team wanted something else that the player could do in order to help sell the elemental mechanic. They proposed what if each element had some kind of special ability/attack. This special would have a cooldown and be very powerful and synergistic with the element and primary attack.

The team went through many ideas for the Slime's special that ranged from more combat oriented to puzzle oriented. We as a team agreed that it should be somewhere in the middle where it has both combat and puzzle uses. An idea thrown out was what if the slime could split itself into two. It wasn't too crazy as the slime was essentially shoot pieces of itself at enemies. This clone of the slime would just be a dummy that made enemies target it instead of the player and be physically present allowing it to interact with pressure plates and the like.

Implementing this dummy seemed straight forward to me. All I had to do was just spawn another player character and that was it or so I thought. This initial idea worked but proved to be unnecessary as there were extra things that the dummy didn't need that were apart of the base character. I ultimately created another actor Blueprint that had everything stripped from it and kept only the essentials.

### Water Element
{% include video.html video="projects/goom/Goom_Water_Prototype.mp4" %}
{% include video.html video="projects/goom/Goom_Water_Final.mp4" %}
This element went through many ideas where we were constantly conflicted on either water or ice. Both trains of thought were interesting from a gameplay perspective. We ultimately decided they could essentially be one where if we made enough progress we could have the water turn into ice. As project continue on, we couldn't get to the ice part and scrapped it as a whole solely going with water.

For the water primary attack projectile, a designer was obsessed with the idea of bubbles/water droplets. They wanted the water droplets to stick to enemies and slow down the attached enemy. The came up with the idea for the special as well where you could detonate your water droplets and having them explode. The idea was really cool but I had no idea how to one stick objects to another thing and have them explode on command.

The first thing I worked on was the detonation mechanic because I felt like this was the easier one of the two. I had already spawned another actor for the Slime special so this probably dealt with somekind of communication between actors. This is where I learned more about events and event dispatchers which I realized where just different names for delegates and callbacks that I had learned about from my CS classes. Once I realized this, it was simple enough to just have the water droplet just bind a callback to the character's special button delegate and when the delegate gets called explode the water droplet.

### Fire Element
{% include video.html video="projects/goom/Goom_Fire_Prototype.mp4" %}
{% include video.html video="projects/goom/Goom_Fire_Final.mp4" %}
A designer wanted the fire element to feel very powerful and slow. Big explosions and lots of AOE damage. Once I had the water droplet explosion working then I applied the same code to the fire projectile. Instead of listening for a callback for when to explode, I would just explode it when it collided with something. This was straightforward and the designer wanted for the special was to have a bomb with a fuse made a huge explosion. I start a timer when it hits something with a callback that calls my explode function. This was exactly what the designer had in mind.

### Electric Element
{% include video.html video="projects/goom/Goom_Electric_Prototype.mp4" %}
{% include video.html video="projects/goom/Goom_Electric_Final.mp4" %}
The designer wanted for the Electric element to make the player feel fast and agile. This element would be about zipping and dashing around enemies. So their initial idea was to have the special be a dash mechanic and have the primary attack somekind of rapid fire projectile. The rapid fire projectile seem straight forward as I had implemented a fire rate onto the primary attack that controlled how fast projectiles could be fired. It seemed like all I had to do was changed this value to be a faster fire rate when you switched to the Electric element. The Electric projectile on the other hand was just the basic Slime one and didn't do anything extra. So I left it as is and continued on to the special dash.

The dash mechanic I didn't know how to do. Some ideas I had were to teleport the player in the direction they were looking. This proposed some problems what if they teleport into a wall what happens then? This didn't seem like the correct way to do this. Another idea I had was to increase the player's move speed by a lot and after a short delay set it back to normal. I liked this idea more and the player could control where they dashed but another problem was still not solved what if they were standing still they would still need to dash in a direction.

Everything I was doing so far didn't involve using any physics in the traditional sense of applying a force and calculating it position based on its acceleration. The Character class had its own Character Movement Component and the Projectile class had a Projectile Movement Component which allows them to move in the world. Upon looking into how I could apply a forces within Unreal, I found out that you can apply forces to physics objects where you specify a vector that combines both direction and magnitude. Applying a force only works on physics objects where the physics engine handles updating the position. The Character Movement Component was the one moving the character and I found out that there was an similar function on the character class called "Launch Character". This worked perfectly where I could just specify a dash force in the direction of the character's velocity if they were moving and just the actor's foward vector when they were standing still.

While designers on my team were focused on figuring out other aspects of the game, I had finished the dash mechanic and wanted to do something more with the Electric projectile. A mechanic that I had seen done a lot in previous games was a chain Electric attack where you could hit multiple targets due them being near each other. When I was trying to figure how to do explosions and apply the appropriate damage, I found out about "Sphere Overlap". I could use this to get all enemies inside a sphere around the hit enemy and find the next closest one. 

In my actual implementation, I used a recursive function to get all enemies around a given point and only add new ones to an array of actors up until a given bounce limit. I was able to draw debug lines in order to show visually what was going on. My teammates really liked the idea and we kept it.

#### Puzzles
Puzzles were going to be a key element of our game and the sense of puzzle solving and exploration something we needed to have working as much as the combat aspects. Not only that but we also wanted to have our elemental system to be integrated with the puzzles as well. I created a Puzzle Interaction Component that handled Activation and Deactivation of the Triggers.

{% include image_no_url.html image="projects/goom/Goom_Interact_Component.png" %}

### Elemental Puzzles
{% include video.html video="projects/goom/Goom_Elemental_Puzzle.mp4" %}
I had previously defined an Elemental State Enum that I used for the player's element. I decided that I could reuse this on puzzle mechanics like a torch for example. Where the torch if it was lit would have the fire element state and none if it was not lit. From here if a water projectile collided with a lit torch then the torch would check to see if the thing that collided with it was a water element if so then it would put the torch out and change its state accordingly.

I reused this idea for a light crystal that could take on any element and only be activated when the correct element was applied. As well as the giving the player a new element when they overlapped a power crystal.

{% include image_no_url.html image="projects/goom/Goom_Elemental_Puzzle.png" %}

### Cinematics
{% include video.html video="projects/goom/Goom_Cinematic_Initial.mp4" %}
{% include video.html video="projects/goom/Goom_Cinematic_Final.mp4" %}
Due to the fact that we were trying to make a puzzle game similar to Zelda and Tunic, in those games you are constantly trapped in a room until you either defeat all enemies or solve the puzzle. Once the room condition is met, it would play some cinematic to let you know that you completed the room and now can leave. We wanted to do something very similar to this. I had some experience with Unreal's Sequencer from my Machinima class where we used the sequencer to sync up animations and dialogue. That was primarily just cinematics and we wanted something in realtime. I knew of games that were able to seamless transition between cinematics and gameplay and tried to research on how I could accomplish something like this. A resource that I found useful was the Unreal Engine's documentation about the sequencer on playing a sequence from Blueprints.

{% include image_no_url.html image="projects/goom/Goom_Cinematic_Prototype.png" %}

Once I found out that I could play a sequence from Blueprints and recalled what I knew about events. I was able to put two and two together in order to get the cinematic to play. First, I made puzzle triggers had a reference to a sequence that I would start playing once the puzzle activated. The puzzle listeners like a door would be activated to close. Second, the puzzle triggers when deactivated would play the cinematic again while the listeners would then deactivate. This worked out really well and we were able to show the player things in the environment that were happening due to their actions.

### Chaos Destruction
{% include video.html video="projects/goom/Goom_Chaos_Initial.mp4" %}
{% include video.html video="projects/goom/Goom_Chaos_Final.mp4" %}
In our game we had explosions from the Fire and Water elements. A designer thought we could do the tried and true thing of having destructibles. Destruction is always fun when the player causes it as well as it could provide puzzles as you might need to use a certain ability to break something in order to progress.

In the same class, there was another Team that had already did destructibles. I had reached out to them about what we wanted to do for our game and asked for some resources that could help me begin. They pointed me to Unreal Engine's Chaos Destruction system. I looked more into it and the system is very complex and powerful. Everything I needed could be done in the engine. Things like: fracturing meshes, placing GeometryCollections, and applying forces in order to break them.

One problem that I ran into was whether or not should destructibles be apart of the health system. I initially made them apart of the health system. If they were then each destructible would need to specify what types of damage it ignores and not allow them to be broken via physics. I felt like this could be a little cumbersome for my designers to use and not fully understand why somethings had physics applied to them and not others. I eventually went with the approach that all destructibles would be broken via physics. This meant that each and every projectile spawned a physics field at their point of impact. Destructibles would only be broken if a force applied was greater than their strain threshold.