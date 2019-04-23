---
layout: post
title: "RogueSharp 4.1.0 Release"
category: release-notes
---
## Version 4.0.0-pre of RogueSharp was just released

### Announcements:

* RogueSharp source code has a new home on Github – https://github.com/FaronBracy/RogueSharp
* Documentation is now generated from source code comments using Docu – https://faronbracy.github.io/RogueSharp/RogueSharp/index.htm
* The v4.1 Nuget package is available – https://www.nuget.org/packages/RogueSharp/4.1.0
* RogueSharp is no longer a PCL library. It has been updated to target .NET Standard 1.0 and .NET Framework 4.0 (or greater)
* The pre-release v4.0 post has additional details and examples. – https://roguesharp.wordpress.com/2017/04/25/roguesharp-4-0-pre-release/

### New features:

* Optimized cave map generator (Thanks to James Neal)
* DijkstraShortestPath.FindPath method which terminates upon finding destination for improved speed (Thanks to flend for this update)
* Added AppVeyor continuous builds (Thanks to Glenn Hoeppner)
* New constructors for **GoalMap** and **PathFinder** to allow for paths which include **diagonal movement**
* **Map.GetCellsInCircle** and **Map.GetBorderCellsInCircle** methods use midpoint circle algorithm to get cells within a radius
* Multiple new “Try” methods which work like their normal counterparts except they return null instead of throwing exceptions
* **Path.TryStepForward** and **Path.TryStepBackward** which will return null instead of throwing a NoMoreStepsException
* **PathFinder.TryFindShortestPath** will return null instead of throwing a PathNotFoundException
* **GoalMap.TryFindPath** will return null instead of throwing a PathNotFoundException

### Breaking changes:

* **Bug fix (4.1.0)** – selecting border **Cells** along edge of map no longer selects center **Cell**
* **Point** and **Rectangle** classes are now **structs** (Thanks to James Neal)
* Updated all the appropriate references to **Cell** with **ICell** (Thanks to Courtney Strachan)
* **Map.ComputeFov** and **Map.AppendFov** both return a **ReadonlyCollection&lt;ICell&gt;** for the cells in the field-of-view instead of returning void.
* The Path returned from **PathFinder.ShortestPath** now includes the **source** cell in the returned Path. This behavior is consistent with how the GoalMap pathfinder works
* **Map.GetCellsInArea** was renamed to **Map.GetCellsInSquare**
* **Map.GetBorderCellsInArea** was renamed to **Map.GetBorderCellsInSquare**
* **Map.GetCellsInRadius** was renamed to **Map.GetCellsInDiamond**
* **Map.GetBorderCellsInRadius** was renamed to **Map.GetBorderCellsInDiamond**