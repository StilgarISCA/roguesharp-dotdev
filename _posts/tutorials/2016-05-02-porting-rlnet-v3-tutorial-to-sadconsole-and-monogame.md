---
layout: post
title: "Porting the Sample RogueSharp V3 Game to SadConsole"
tagline:
category: [tutorials, v3, SadConsole]
---
A few weeks ago I spent some time porting the RogueSharp sample game from RLNET to SadConsole.

![alt text](/img/sadconsoleport.png "Screenshot of running sad console port")

### Source Code

The full source code for this can be found on BitBucket:
[https://bitbucket.org/FaronBracy/roguesharpsadconsolesamples/src](https://bitbucket.org/FaronBracy/roguesharpsadconsolesamples/src)

### Notes from the porting process

Because it was a direct port I’m sure I did some things that were undesirable. What follows are some of my notes from the porting process:

RLNET has a concept of a root console. You could draw directly to the root console or you could also draw to sub consoles and then Blit them to a specific locations of the root.

For SadConsole the examples from ThakraAndy show making your own console classes that inherit from SadConsole’s Console class. I didn’t do that however. I just got rid of the RLNET root console and made each of the sub consoles a new SadConsole.Console in my main Game class. This then means that I pass around references of those consoles to each of my object’s draw methods. For example the Player class’s draw method has a reference to both the map console and the stats console and draws directly to them. It doesn’t seem like this is the intended way to work with consoles however.

I also saw that SadConsole has the idea of an Entity which from what I can tell is independent of all of the regular console cells. It seems like the proper thing to do would be to have the player, monsters, and anything that moves around on the map be an entity, so that I wouldn’t have to keep track of what is under the entity and draw it back after the entity moves.

RLNET didn’t have the same entity concept and since I was doing a straight up conversion with as few changes as possible I actually did not use the SadConsole.Entity class. Basically I keep track of when anything changes (due to player input or monsters moving on their own) and then clear all the consoles cells completely and redraw them based on the current state of the game and game objects. Most likely a bad idea, but it worked.

I couldn’t figure out how to change the background color of a range of cells (health bars for the monsters, and refresh bars for abilities) so I made my own extension method which went through the cells one at a time and changed the background color. I think there must be a better way to do that, but I couldn’t find it right off.

Additionally SadConsole has it’s own way of handling input which I completely ignored and just used standard MonoGame input handling. This was due to time constraints and me being lazy. I plan to look over how SadConsole handles input more closely in the future.

Overall the port went quickly and required far fewer changes than I expected.
