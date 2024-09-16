---
layout: post
title: 'Ivorfall'
---

##### Role - Lead Gameplay Programmer (Team of 26)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - C++/Blueprint Workflow, Interdisciplinary Teamwork, Communication, Development Cycle

{% include youtube.html url="https://www.youtube.com/embed/T3faHrBHFx8" %}

Ivorfall is a EAE Capstone project at the University of Utah for the year 2023-2024. Ivorfall is a isometric twinstick roguelike shooter where you play as a trigger happy detective trying to solve a mystery set in a steampunk city. The gameplay is inspired by games like Hades, RUINER, and Doom. The prototype that was made in 2 weeks with a team of 13 in order to demonstrate the capabilities of both the team and prove the game's concept to the professors and an industry panel. This was 1 of 16 prototypes where only 8 of them would be greenlit for full production for the rest of the school year. Upon being greenlit for begin full production, the team's size doubled to 26.

{% include image.html url="https://store.steampowered.com/app/2901040/Ivorfall/" image="projects/ivorfall/Ivorfall_Splash.png" text="Steam"%}

I loved my time on Ivorfall, particularly working on the Weapon Modification System. This system was central to the gameplay experience in Ivorfall. Players constantly picked up different modifications, making it a key part of the gameplay loop. Designers had a greate time creating fun and unique combinations that made players feel powerful sometimes too powerful. The full school year of development allowed me and the team to refine and polish both the gameplay mechanics and the underlying implementation. The prototype for the system was assembled and functional, but wasn't ready for further expansion at that stage.

{% include video_sbs.html video1="projects/ivorfall/Ivorfall_WeaponMods_Initial.mp4" video2="projects/ivorfall/Ivorfall_WeaponMods_Final.mp4" %}

Originally, the system was designed to be extremely flexible, allowing modifications to be stackable and enhance the pistol in various ways. However, during the first few playtests, players felt like the modifications (mods) were too tedious to pick up and lacked impact. Design and Programming got together to discuss redesigning the system to provide a better sense of power for the player. We ultimately decided to go with a two-mod class design: Primary and Secondary. A Primary class mod function like a barrel modification, permanently altering how the weapon fired. A Secondary class mod would be more akin to ammo/bullets, such as oil pellets, electricity, and rockets. Implementing this design was relatively straightforward due to the separation. A Primary mod would define the projectile's movement and speed while the Secondary mod would determine the type of damage/effects upon impact. I needed to only ensure that there was one of each mod class equipped at a time and when the Secondary ran out of ammo then it would be unequipped.

We had biweekly playtests with the rest of the Capstone class for the school year, during which we received continous feedback on the game. Based on this feedback and footage of players interacting with the game, it became clear that the mods still lacked impact. Players weren't immediately drawn to the mods, as they seemed to be a nice addition rather than a necessity. The team wanted mods to feel more powerful and sometimes a must have. Design proposed an idea: what if each unique pairing of a Primary and Secondary mod gave the weapon an entirely different behavior? This approach was eventaully adopted and shipped with the game. Primary Mods effectively became distinct weapon types, while Secondary Mods in combination with a Primary Mod, altered not only the projectile but also its firing mechanics and additional effects.

In the final implementation, I used a hash map to map a Primary mod class to one of the possible Secondary mod classes. When the Player equips a Secondary Mod
the Primary Mod looks up the appropriate combination mode in the table. These combination modes were implemented as a separate class from the Weapon class, handling the internal logic whether or not it spawned a projectile or raycast for hits. The Weapon class managed the ammo and let the combination mode to execute its logic.

{% include video.html video="projects/ivorfall/Ivorfall_WeaponMods_Shipping.mp4" %}

For example the flamethrower combination is sweeping the area where the fire is supposed to be on an interval for targets. If targets are found then raycasts are sent out to each target to ensure the player does not applying damage through walls. The oil artillery shot is spawning a projectile that follows a parabolic trajectory and upon impact activates a sphere used for collisions waiting for overlap events. Upon receiving an overlap event it will apply a slowing damage effect to the target for as long as it is overlaping the oil.

As a Lead on the project, I was the main programmer responsible for implementing the mods system above. I also taught other programmers and designers on how to use the system and create new combinations. Additionally, I assisted the AI engineers who wanted to adapt the mods system for enemies but without the combination aspects. I worked closely with artists to ensure that their work was properly implemented and functioned as  intended. Furthermore, I attended meetings with producers to provide updates on progress of the system. It was a rewarding experience working collaboratively, and I am proud of what we ultimately shipped.

One other thing I was deeply involved with on the project was the interaction system, which allowed players to equip mods by pressing a button when near one. I created an actor component that scanned a radius around the player on a timer to find the closest interactable object. When a valid interactable was found, the component broadcasted a delegate to notify other systems that the player could interact. Interactables implement an interface that allow them to be interacted with by an interactor. Although this idea got scrapped, we initially considered allowing AI characters to equip mods in the same manner as the player. The AI would have participated in the interaction system, but with one difference being that they require an component to do so. The AI would rather have their behavior detect for a nearby interactable and navigate towards it before calling interact on it. The interactable would then peform its own logic handling that the interactor was potentially an AI character.

{% include video.html video="projects/ivorfall/Ivorfall_Shop_Shipping.mp4" %}

Branching off of the interaction system, we developed a shop interactable, which allows players to interact with a shop item and attempt to purchase it if they have enough money. The shop has a configurable list of items that it can offer. Using this list, the shop randomizes its offerings while considering the player's current state in order to avoid duplicates. The delegate broadcasted upon finding an interactable is used in the UI to display information about the item. Shop items were created with a data driven approach, meaning there is only one shop item class. The items are configured based on spreadsheet specifices the mesh, materials, cost, and player modifications associated each item. The shop item's interaction involves checking whether the player can afford them. If so, the cost is deducted, and the desired modifications are applied to the interactor. Designers could easily view all item costs at a glance and easily do the setup new items. Artists could update the mesh, knowing that changes would be reflected through the entire game due a single source truth.