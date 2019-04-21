---
layout: post
title: "RogueSharp 1.1 Released – Cave-Like Map Creation Using Cellular Automata"
category: release-notes
---

![alt text](/img/cavecreationsamples.gif "Rotating image of generated cave maps")

## RogueSharp Version 1.1

Version 1.1 of RogueSharp was just released. It now includes a new map creation strategy for creating cave-like maps based off of an article on RogueBasin with some custom modifications.

* RogueSharp v1.1 Nuget Package
* RogueSharp v1.1 Source Code

### Usage

{% highlight csharp %}
IMapCreationStrategy<Map> mapCreationStrategy =
   new CaveMapCreationStrategy<Map>( 50, 30, 45, 4, 2 );
IMap map = Map.Create( mapCreationStrategy );
Parameters
{% endhighlight %}

### Parameters

* **width** – The width of the Map (in Cells not pixels) to be created
* **height** – The height of the Map (in Cells not pixels) to be created</param>
* **fillProbability** – Recommend using values between **40** and **60**. Percent chance that a given cell will be a floor when randomizing all cells before starting the cellular automata algorithm.
* **totalIterations** – Recommend using values between **2** and **5**. Number of times to execute the cellular automata algorithm.
* **cutoffOfBigAreaFill** – Recommend using value less than **4**. The iteration number to switch from the large area fill algorithm to a nearest neighbor algorithm
* **random** (optional) – A class implementing IRandom that will be used to generate pseudo-random numbers necessary to create the Map. If this parameter is omitted then a default implementation of .NET’s System.Random will be used.