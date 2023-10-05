---
layout: post
title: 'Inquiry of Ivorfall'
---

##### Role - Lead Gameplay Programmer (Team of 13)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - Communication, Rapid Prototype/Ideation, Interdisciplinary Teamwork

{% include youtube.html url="https://drive.google.com/file/d/1agWnFlMo954uTNUArOCsGdJzpVaD03xh/preview" %}

Inquiry of Ivorfall is a EAE Capstone project at the University of Utah for the year 2023-2024. Inquiry of Ivorfall is a topdown isometric twinstick shooter where you play as a trigger happy detective trying to solve a mystery set in a steampunk city. The gameplay is inspired by games like Hades, RUINER, and Doom. This is a prototype that was made in 2 weeks with a team of 13 in order to demonstrate the capabilities of both the team and prove the game's concept to an industry panel. The panel of judges alongside the professors voted on which 8 of 16 prototypes would be greenlit for full production for the rest of the semester. This project was greenlit to begin full production in which the team's size is expected to double. I was overburden with joy when I found out that the project was greenlit. I really enjoyed prototyping the gameplay for this project. We were so tight on time and I ended up taking many shortcuts. I am planning on fixing and properly implementing the mechanics in order to be more robust and versatile.

{% include image.html url="https://drive.google.com/file/d/1wzAMU_GvxPgW8HV4SfApDXnZxomK6l_M/view?usp=drive_link" image="projects/inquiryofivorfall/InquiryOfIvorfall_Splash.png" %}

For the prototype, I was tasked with creating player mechanics. Things like shooting the gun, equipping weapon mods, twinstick controls, and worked with VFX and SFX artists to implement their work onto these mechanics. Everything was done in Blueprints in order to rapidly iterate on ideas and mechanics as this was primarily a proof of concept. An example of this was that I worked inside my own gymnasium level where I could test the mechanics independently of other teammates working inside the main prototype level.

{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponMods_Initial.mp4" %}
{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_WeaponMods_Final.mp4" %}

I had the equipping and shooting mechanics working very early on. Weapon mods would equip themselves to the weapon and cause the weapon to fire differently. For example, the bounce mod which fires a cog wheel bounces off walls has a unique projectile while the blunderbuss is a box trace in the forward direction from the weapon's firing tip. Each weapon mod has parameters that can allow for designers in the future to easily make new types the same can be said for the projectiles.

{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Mod_Details.png" %}

I exposed variables that the artist could set in order to spawn/play the appriopriate effects when certain gameplay events happen. This allowed for the artist to work and test their art simulatenously on the actual game mechanics while I was still implementating other features.
{% include video.html video="projects/inquiryofivorfall/InquiryOfIvorfall_VFX_Implementation.mp4" %}
{% include image_no_url.html image="projects/inquiryofivorfall/InquiryOfIvorfall_Projectile_Details.png" %}

