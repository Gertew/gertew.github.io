---
layout: post
title: 'Project Eon'
---

##### Role - Lead Gameplay Programmer (Team of 3)
##### Engine/Platform - Unreal Engine 5 (Windows)
##### Practiced Skills - C++, Procedural Generation, Leadership, Rapid Prototype

{% include youtube.html url="https://drive.google.com/file/d/15MMTjCtxmPDkTBxgtFa_-x-Usw_lT-6Z/preview" %}

Project Eon is a Character Action Dungeon Crawler where the player picks a hero from a cast of characters ranging across time in order to save the universe. This project is inspired by games like League of Legends, Super Smash Bros, Hades, and Enter the Gungeon where it has unique Heroes, gameplay altering items, and a procedurally generated dungeon.

{% include image.html url="https://drive.google.com/file/d/1f_QrxpTGNix3DJnH0_eyeiemKIsXvRTg/view?usp=sharing" image="projects/inquiryofivorfall/InquiryOfIvorfall_Splash.png" text="Demo"%}

Procedurally generated content has always seemed mysterious to me and how it is used in games. For this project I wanted to attempt to better understand and try implementing it for myself. The general idea about procedurally generating something is that generating something more than once should not result in the exact same thing. This only applies if the seed for the generator is random. Trying to do research about this topic led me to having to first understand more about how computers generate random numbers. Previously I had believed and understood was that generating a random number was completely random and there is no way to predict the next number. I took a look at the C++ standard library and Unreal Engine documentation for generating a random number I encountered a key word "pseudo" upon reading this word I became confused at first then enlightened. The random numbers being generated are in a sequence but since the sequence might be extremely long it is hard to detect when it repeats. This led me to understand that the generator must be able to generate the same sequence again because otherwise it would be completely random and no longer pseudo. It made me immediately think of a hash function that when given some input will always produce the same output. This is where the seed part of the randomness comes into play. The generator is just an algorithm that returns an output and the seed controls the seqeunce produced. I did find out that there are algorithms for completely generating a random number but the commonly used ones are pseudo random.

This helped me completely understand why games like Minecraft allows players to enter a world seed for the generator that allows for the same world to be generated exactly. Now that I had understood how seeding a random number generator and the numbers I was getting from the generator would always be the in the same sequence. I began work on implementing the Dungeon generation. First I wanted a representation of the Dungeon that would be easy to visualize so I choose a 2 dimensional grid. I could easily display the grid as symbols with a different symbol representing a different room in the Dungeon. The grid would have some width n and some height m with x amount of rooms being connected.

The general idea is to pick an initial location randomly inside of the grid and then afterwards pick a random adjacent cell that hasn't been picked until there no more rooms that can be picked. In the event of a dead end then it will pick a different cell to look at from the current list of picked cells. This current implementation works and takes on the order of the number of rooms to be generated x. In the worst case it will have to examine the entire n by m grid.

An example 4x4 Dungeon with 4 rooms could have the following generation where 1s represent a generated room and 0s are empty
0001
0011
0010
0000

I would continue to expand this generator to factor in different types of rooms by making use an enumerator to define all possible types of rooms and add constraints like limited number of a certain type. The constraints would be a hash map keeping track of room types generated so far and the desired mapping and remove the room type from being generated if the desired amount has already been met. I would use bitmasking in order to remove rooms that could no longer be generated. If all room types would to be removed then the generation is completed.

An example 5x5 Dungeon with 4 rooms with types 2 type ones, 1 type two, and 1 type three could have the following generation
00000
00000
00321
00100
00000

Implementing unique Heroes was the first challenge that the team and I first had to tackle. How could be design characters that share a common set of abilities but be completely unique in other aspects. Using Inheritance and creating an Abstract base character class would be a good place to start. I created a base class that all characters inherit from that both Player and AI controllers would be able to use. This class defined actor components that would be necessary and provides methods for accessing game specific variables.

How would the characters be able to have unique abilities? This was challenging in the sense that we wanted these abilities to potentially be reused especially for AI characters. Each unique character class could go ahead and define their abilities right there in the class but making it reusable wasn't possible with this approach. Taking a step back and thinking about what is an ability and looking at how other games did something similar espically in the MOBA/Hero space. Abilities essentially boil down to being a completely separate class that encapuslates everything and anything the ability needs in order to function. Characters would go and activate these abilities and let them do whatever they needed to. Abilities range from very simple like playing an animation to more complex like preventing character movement and doing ray casts to detect targets in front of the character.

This concept of an ability is common in games and I had previously used the Ability System provided by the Unreal Engine on a previous project Ivorfall for weapon firing due multiple designers need to change weapon behavior indepedently of one another. AI characters also in Ivorfall didn't need to have their abilities be reused by potentially other characters as we had a limited amount of enemy types so inheritance worked fine here. Looking at the implementation of the Ability System and researching on how Epic used it on their own projects. It started to click and make more sense on how I could adapt the system to be used for Characters and Items on this project.

Incorporating a Health System is supported by the Ability System through the use of an Attribute Set that defines attributes and allows for modifiers to be applied to said attributes. I defined a Health attribute that would allow for health to be manipulated and broadcast a delegate letting anyone who cares knows about when health changes and when it runs out.

The Ability System provided hooks allowing us to create and implement Items in a generic way that were usable on all characters. Items would need to only know who is trying to activate the ability and perform any logic needed for the specific Item. I was able to design an Item that reacted to when the Player kills an Enemy that caused an explosion around the Enemy. I would listen for the death event to happen that was caused by the Player and then I would sweep a sphere around the Enemy in order to see if there were any nearby targets. If so then I would apply a negative health modifier to the target's Health Attribute Set.
