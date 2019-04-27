---
layout: post
title: "RogueSharp Tutorial Series Ported to Unity"
tagline: "Guest Blog Post"
category: news
---
## Introduction

I’m excited that one of the visitors to the blog took it upon himself to port the code from the RogueSharp tutorial series to Unity. I asked if he would be interested in writing a guest blog post and he agreed! A huge thank you to Brian for all the hard work with the port and for the  information posted below.

– Faron

### About the Author

First off, thank you Faron Bracy for developing the **RogueSharp** library and all the time you’ve spent developing the library and writing tutorials on it!

I have been developing with Unity for some time now and have been looking for a C# Rogue or 2D RPG library. There a couple Rogue/RPG libraries on the Unity asset store but the libraries seem to be too integrated or reliant on Unity. I was looking for a pure C# implementation and something I could work into Unity but change graphics engine later if I wanted. Thus **RogueSharp**.

### About the Port to Unity

I developed a port of Faron’s RogueSharp v3 tutorial series with Unity except I ended up using RogueSharp V4! I originally went through the entire tutorial series using RLNet Console, which gave me a comfortable feel for the library and how I wanted to approach integrating with Unity. With the Unity integration and some of my own personal preferences, I’ve made a numbers changes to the original tutorial for the Unity port. Some of notable changes:

1. Uses Unity (version 2018.2.1f1) instead of RLNet Console, so no more RLColor or RLConsole calls;
1. The code has been reorganized mostly in a MVC pattern. There are no dependencies in the Model and Controller logic against Unity. Only View logic has any reliance on Unity. This could make ports on other consoles/backends easier (maybe?);
1. Uses diagonal movement in RogueSharp v4 including monster pathfinding;
1. Most static references removed, passes data by reference. Personal preference, I’m not big on lots of static functions;
1. Uses RogueSharp v4, not v3 as in the original tutorial series. Only caused a few minor changes to the tutorial code;
1. Object pooling used from Catlike coding,https://catlikecoding.com/unity/tutorials/ for Unity GameObjects that represent cells on the console. This was necessary as the height/width sizes of the console cause too many GameObects to be created (thousands). This caused significant slowdowns in the FPS, so I needed an optimization early in the process. As a result, the View logic will only display tiles/cells that are visible in the Camera and dynamically change the viewable tiles/cells as the player moves. This allows for arbitrarily large maps;
1. There are few odds and ends added to the code that are placeholders for a future implementation that allows switching between ASCII characters and graphical tiles.
1. The port can be found at: https://github.com/Olivexe/RogueSharpTutorialUnityPort. Along with the project source code, I’ve put in a Unity Package file, named RogueSharpTutorialPort.unitypackage. You should be able to import the package file into a new Unity Project, click the sample scene, and hit play. It should play exactly as the tutorial. There is now a second branch in my port that I am slowly porting the remainder of the complete RogueSharp tutorial over to Unity.

Thank you,

Brian "Olivexe" Pleffner


