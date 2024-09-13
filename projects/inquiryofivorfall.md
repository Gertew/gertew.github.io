---
layout: post
title: 'Inquiry of Ivorfall'
---

##### Role - Lead Gameplay Programmer (Team of 26)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - C++/Blueprint Workflow, Interdisciplinary Teamwork, Communication, Development Cycle

{% include youtube.html url="https://www.youtube.com/embed/T3faHrBHFx8" %}

Ivorfall is a EAE Capstone project at the University of Utah for the year 2023-2024. Ivorfall is a isometric twinstick roguelike shooter where you play as a trigger happy detective trying to solve a mystery set in a steampunk city. The gameplay is inspired by games like Hades, RUINER, and Doom. The prototype that was made in 2 weeks with a team of 13 in order to demonstrate the capabilities of both the team and prove the game's concept to the professors and an industry panel. This was 1 of 16 prototypes where only 8 of them would be greenlit for full production for the rest of the school year. Upon being greenlit for begin full production, the team's size doubled to 26.

{% include image.html url="https://store.steampowered.com/app/2901040/Ivorfall/" image="projects/inquiryofivorfall/InquiryOfIvorfall_Splash.png" text="Steam"%}

Ivorfall was originally known as Inquiry of Ivorfall and upon rebranding of the game with hopes of better marketing exposure for the game, the team shortened the title to just Ivorfall. Its internal working name is known to be Inquiry or IoI where many files still contain one of this short hand names.

For the prototype, I was tasked with creating player mechanics and getting the isometric twinstick to work. Things like shooting the gun, equipping weapon mods, twin-stick controls, and working with VFX and SFX artists to implement their work onto these mechanics. Initially everything was done in Blueprints in order to rapidly iterate on ideas and mechanics as this was primarily a proof of concept. An example of this was that I worked inside my own gymnasium level where I could test the mechanics independently of other teammates working inside the main prototype level.

{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponMods_Initial.mp4" %}
{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponMods_Final.mp4" %}

I had the weapon mod and shooting mechanics working very early on. Weapon mods would equip themselves to the weapon and cause the weapon to fire differently. For example, the bounce mod which fires a cog wheel bounces off walls has a unique projectile while the blunderbuss is a box trace in the forward direction from the weapon's firing tip. Each weapon mod has parameters that can allow for designers in the future to easily make new types the same can be said for the projectiles.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Mod_Details.png" %}

I exposed variables that the artist could set in order to spawn/play the appropriate effects when certain gameplay events happen. This allowed for the artist to work and test their art simultaneously  on the actual game mechanics while I was still implementing other features.
{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_VFX_Implementation.mp4" %}
{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Projectile_Details.png" %}

The prototype had many flaws and I had made many assumptions in order to get something working in order to make the prototype deadline. Assumptions that I made were that weapon mods were just slight number tweaks from one another. Another assumption that I made was that only the player could be the one to equip these mods. I was not thinking about having enemies interact with the system at the time either. I bring up these points as going beyond the prototype phase and into alpha and beta stages. I knew I would need to face these problems.

Alpha stage.
The alpha stage was the hardest in terms of having to handle the team size doubling. This resulted in longer and more spread out meetings in order to bring up to speed our new teammates on both the game's current state and work that needed to be done. The onboard process was an extreme headache because many newcomers came from different teams all which potentially used a different game engine than Unreal. I met frequently with the new engineers in order to gauge their experience with the engine and bring them up to speed on systems that I had wrote.

Now for the more technical stuff. In the first couple of meetings with the entire team, we all presented ideas where each of us thought where the came could go. One idea that we all agreed upon was that the weapon mods weren't really unique between one another. The player isn't making decisions on which mod to get but instead picking up any they can find. In order to combat this problem, the team liked the idea of making mods more unique and giving archetypes to them. Archetypes like bounce, multi-target, and area-of-effect were the main ones that we decided to first work on.

In order to convert the mods from their generic state, I added way to identify the types of mods using an enumeration. Mods could either be a primary or secondary mod where a primary mod dictates how the changes the weapon's firing capabilities while a secondary mod changes the types of bullets/effects that would be applied. This resulted in not every mod can be combined with another mod. This helped scope down the number of possible combinations we would need to support where it becomes now only the cross product between primary and secondary instead of the number of mods squared.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Updated_Mod_Details.png" %}

The previous system resulted in a monolithic base class that had every feature possible and not all the children would need some of these features. I went back and revised my implementation in order to better use inheritance to make sure that child classes were the one implementing and doing more unique stuff while the parent held any common functionality need between all of them. Mods and Projectiles are now separated where mods dictate their interactions with the weapon and choose which projectile or trace effect to be used. Projectiles define their own behavior on how they move in the world but their damage is dictated by the mod.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Updated_Projectile_Details.png" %}

I was able to move a lot of the heavy Mod and Projectile logic out of Blueprints and into C++ which made the Blueprint Graph more readable and simplier for Designers to work with. As well as keeping core logic out of Designer's hands who could potentially break stuff by not properly plugging in things inside the Blueprint Editor. By making use of UFUNCTION and UPROPERTY macros to expose C++ functionality as need.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Mod_Header.png" %}

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Projectile_Header.png" %}

Through the use of the Gameplay Ability System where Gameplay events are separated into 3 ideas: Gameplay Abilities, Gameplay Effects, and Gameplay Cues. The separation of concerns here allows Gameplay Abilities to trigger Gameplay Effects and Gameplay Cues allowing for multiple abilities to trigger the same effects and cues. This reusability and modularity of the system is what I am now using to handle Mod combinations.

Each Mod combination has its own Gameplay Ability derived from a C++ base class that defines functionality to spawn projectiles, do collision traces, and apply forces/knockback that allows it to trigger its own unique effects and cues. Where the majority of Gameplay Abilities are created in Blueprints which then dictate the type of functionality and behavior that the Designers want.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponFire_Header.png" %}

Some combinations reuse the same effects like the Blunderbuss and Oil Combination which acts like a traditional flamethrower that has a damage over time effect which is reused by the Sniper and Oil Combination that launches a mortar grenade that explodes applying a damage over time effect to all enemies. The only difference between the two is the amount of damage dealt which boils down to a different number. For others it must just be that its spawning a different type of projectile.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponFire_GameplayAbility.png" %}

The powerfulness of this system allows for abilities to be defined in its own separate place and the mods just contain a map of other Mod types to Gameplay Abilities. The functionality is not directly tied to the mod and can be easily switched out without having to cut/delete many lines of code from the Mod object. It is also extremely Blueprint and Designer friendly by using ScalableFloats that have an underlying CurveTable. The CurveTable especially the linear type is very similar looking to a spreadsheet and Designers can adjust and balance numbers very easily by just changing those values and testing inside the editor.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_ModDamage_Table.png" %}

Another bonus of using Gameplay Abilities is that not only can the player use the Weapon Mods but so can the enemies. By separating the functionality into its own separate ability, I can either reuse the same ability if only numbers need to be different or make use of inheritance to create a different ability all while just specifying on the Mod which type of character is equipping the mod. This allowed our game initially from just having 1 type of enemy to having 6 additional ones, one for each Mod type. I have been closely collaborating with the engineer implementing the enemy AI and we were able to have the enemies behave different based on the Mod type as well.