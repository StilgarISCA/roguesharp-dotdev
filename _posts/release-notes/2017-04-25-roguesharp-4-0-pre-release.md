---
layout: post
title: "RogueSharp 4.0 Pre-Release"
category: release-notes
---
## Version 4.0.0-pre of RogueSharp was just released

In order to obtain pre-release versions via Nuget make sure to choose "Include prerelease" packages from the Nuget Package Manager.

![alt text](/img/prereleasenuget.png "NuGet Package Manager screenshot")

* [RogueSharp v4.0-pre Nuget Package](https://www.nuget.org/packages/RogueSharp/4.0.0-pre)
* [RogueSharp v4.0-pre Source Code](https://github.com/FaronBracy/RogueSharp/releases/tag/v3.0-pre)

Thank you to numerous contributors for this release including James Neal, flend, Glenn Hoeppner, and Courtney Strachan. Any feedback regarding this pre-release version is appreciated!

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

* **Point** and **Rectangle** classes are now structs (Thanks to James Neal)
* Updated all the appropriate references to **Cell** with **ICell** (Thanks to Courtney Strachan)
* **Map.ComputeFov** and **Map.AppendFov** both return a **ReadonlyCollection&lt;ICell&gt;** for the cells in the field-of-view instead of returning void.
* The Path returned from **PathFinder.ShortestPath** now includes the **source** cell in the returned Path. This behavior is consistent with how the GoalMap pathfinder works
* **Map.GetCellsInArea** was renamed to **Map.GetCellsInSquare**
* **Map.GetBorderCellsInArea** was renamed to **Map.GetBorderCellsInSquare**
* **Map.GetCellsInRadius** was renamed to **Map.GetCellsInDiamond**
* **Map.GetBorderCellsInRadius** was renamed to **Map.GetBorderCellsInDiamond**

### Pathfinder Diagonal Path Example:

By using the new constructor on the **PathFinder** class and providing a second argument which is the cost of diagonal movement you can now return paths that consider diagonals.

{% highlight csharp %}
public Path FindPath( ICell source, ICell destination, IMap map )
{
  // 1.41 is the cost of diagonal movment compared to horizontal or vertical of 1
  var pathFinder = new PathFinder( map, 1.41 );

  // TryFindShortestPath is a new method that will return null
  // instead of throwing PathNotFoundException if there isn't a path.
  return pathFinder.TryFindShortestPath( source, destination );
}
{% endhighlight %}

![alt text](/img/diagonalpathfinder.gif "Animated gif of diagonal pathfinder")

### GoalMap Diagonal Path Example:

The **GoalMap** class also received a new constructor which will allow it to use diagonals.
{% highlight csharp %}
/// Constructs a new instance of a GoalMap for the specified Map
/// that will consider diagonal movements to be valid if allowDiagonalMovement is set to true.
/// "map" The Map that this GoalMap will be created for
/// "allowDiagonalMovement" True if diagonal movements are allowed. False otherwise
public GoalMap( IMap map, bool allowDiagonalMovement )
{% endhighlight %}

![alt text](/img/diagonalgoalmap.gif "Animated gif of diagonal goal map")

### Map.GetCellsInCircle Example:

RogueSharp has always had a lot of different [Cell selection methods as seen in this old post](/tile-and-cell-selection-with-roguesharp/).

![alt text](/img/selection.gif "Animated gif of working cell selection")

What it has been missing is a good method for getting cells in a Circle. We now have two new methods which will help with this.

{% highlight csharp %}
/// Get an IEnumerable of Cells in a circle around the center Cell up
/// to the specified radius using Bresenham's midpoint circle algorithm
/// "xCenter" X location of the center Cell with 0 as the farthest left
/// "yCenter" Y location of the center Cell with 0 as the top
/// "radius" The number of Cells to get in a radius from the center Cell
public IEnumerable<ICell> GetCellsInCircle( int xCenter, int yCenter, int radius )

/// Get an IEnumerable of outermost border Cells in a circle around the center
/// Cell up to the specified radius using Bresenham's midpoint circle algorithm
/// "xCenter"X location of the center Cell with 0 as the farthest left
/// "yCenter"Y location of the center Cell with 0 as the top
/// "radius"The number of Cells to get in a radius from the center Cell
public IEnumerable<ICell> GetBorderCellsInCircle( int xCenter, int yCenter, int radius )
{% endhighlight %}

![alt text](/img/circleselection.gif "Animated gif of circle selection")

Using the new circle selection code it is possible to create poor implementation of circular field-of-view even though RogueSharp doesn’t have native support for it. Here is an example of some code that will do it.

{% highlight csharp %}
private static IEnumerable<ICell> GetFieldOfView( int x, int y, IMap map )
{
  List<ICell> circleFov = new List<ICell>();
  var fieldOfView = new FieldOfView( map );
  var cellsInFov = fieldOfView.ComputeFov( x, y, (int) ( _selectionSize * 1.5 ), true );
  var circle = map.GetCellsInCircle( x, y, _selectionSize ).ToList();
  foreach ( ICell cell in cellsInFov )
  {
    if ( circle.Contains( cell ) )
    {
      circleFov.Add( cell );
    }
  }
  return circleFov;
}
{% endhighlight %}

It will create field-of-view that looks like this:

![alt text](/img/circularfieldofview.gif "Animated gif growing and shrinking view fields on a map")

