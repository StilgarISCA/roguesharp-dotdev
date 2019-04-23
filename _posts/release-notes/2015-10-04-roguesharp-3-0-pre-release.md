---
layout: post
title: "RogueSharp 3.0 Pre-Release"
tagline: "Goal Maps"
category: release-notes
---
## Version 3.0.0-pre of RogueSharp was just released

This is considered a pre-release version. In order to obtain it via Nuget make sure to choose “Include Prerelease” packages from the Nuget Package Manager.

![alt text](/img/includeprerelease.png "Include prerelease in NuGet Package Manager image")

Any feedback regarding this pre-release version is appreciated!

* [RogueSharp v3.0-pre Nuget Package](http://www.nuget.org/packages/RogueSharp/3.0.0-pre)
* [RogueSharp v3.0-pre Source Code](https://github.com/FaronBracy/RogueSharp/releases/tag/v3.0-pre)

### New features:

* Added Path class to provide a type with more functionality than simple IEnumerable
  * Returned from GoalMap and PathFinder classes
* Reworked GoalMap interface to be more consistent with Pathfinder class
  * Ability to add and remove obstacles
  * Ability to remove Goals once they are added
* Numerous bug fixes to GoalMap class
  * Fixed possible infinite recursion in some edge cases
  * Avoidance algorithm now will never choose  a Path through a Goal that is being avoided
* FxCop clean up

### Breaking changes:

* Pathfinder now returns Path type instead of IEnumerable
* GoalMap now returns Path type instead of IEnumerable
* Map creation algorithms moved to their own namespace
* DiceNotation “Type” renamed to “TermType” to avoid being a keyword

### GoalMap / Path Example:

{% highlight csharp %}
// Create a new map that only has a wall around the border
var map = Map.Create( new BorderOnlyMapCreationStrategy
<Map>( 50, 50 ) );

// Create a new GoalMap for the map
var goalMap = new GoalMap( map );

// Add a Goal at X=10, Y=25 with a weight of 10
// Lower weights are more desirable
goalMap.AddGoal( 10, 25, 10 );

// Add a second Goal at X=40, Y=15 with a weight of 10
goalMap.AddGoal( 40, 15, 10 );

// Add an obstacle at X=1, Y=2
// An obstacle is something that a Path must not go through
// Examples: an ally, an enemy, water, etc.
goalMap.AddObstacle( 1, 2 );

// Find a "best" path to a Goal based on Goal weight and distance to the Goal
// Start the path from X=1, Y=1
// The path will not pass through obstacles
Path path = goalMap.FindPath( 1, 1 );

// Get the number of steps in the path
// Output = 34
Console.WriteLine( path.Length );

// Get the start of the path
// Output = 1:1
Console.WriteLine( "{0}:{1}", path.Start.X, path.Start.Y );

// Get the end of the path
// Output = 10:25
Console.WriteLine( "{0}:{1}", path.End.X, path.End.Y );

// Take a step forward along the path
Cell nextStep = path.StepForward();
// Output = 2:1
Console.WriteLine( "{0}:{1}", nextStep.X, nextStep.Y );

// Take a backward along the path
Cell previousStep = path.StepBackward();
// Output = 1:1
Console.WriteLine( "{0}:{1}", previousStep.X, previousStep.Y );

// Iterate through each Step along the Path
// Output = 1:1 - 2:1 - 2:2 - 2:3 - 2:4 - 2:5 - 2:6 - 2:7 - 2:8 - 2:9 - 2:10 -
// 2:11 - 2:12 - 2:13 - 2:14 - 2:15 - 2:16 - 2:17 - 2:18 - 2:19 - 2:20 - 2:21 -
// 2:22 - 2:23 - 2:24 - 2:25 - 3:25 - 4:25 - 5:25 - 6:25 - 7:25 - 8:25 - 9:25 - 10:25 -
foreach ( Cell step in path.Steps )
{
   Console.Write( "{0}:{1} - ", step.X, step.Y );
}
{% endhighlight %}
