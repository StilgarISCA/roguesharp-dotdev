---
layout: post
title: "Using RogueSharp with RLNET Console"
tagline: "v2 RLNET Tutorials"
category: [tutorials, v2, RLNET]
---
People have asked me if it is possible to use RogueSharp without using MonoGame. They’ve also asked how to use RogueSharp to make a classic ASCII game. Though MonoGame is a great framework there are plenty of other ways to create games in C#. Today I’m going to show how to use RogueSharp with [RLNET](https://bitbucket.org/clarktravism/rlnet) which is a lightweight API to help quickly create tile-based games for .NET.

![alt text](/img/largecavern.png "Large Cavern")

### Goal

In this tutorial we’ll get RogueSharp and RLNET added to a brand new project. We’ll render the console which will be a cave-like map and intercept keyboard commands to be able to move the player represented by an ‘@’ through the ASCII cave. We’ll calculate field-of-view and render what the player can see as well as what they have previously explored. Best of all we’ll do this in just a few lines of code.

### Creating a new Console Project

I’ll be using Visual Studio for this Project.

1. Go to the **File** menu and choose **New -> Project…**
1. From **Templates** choose **Visual C# -> Windows Desktop -> Console Application**
1. Give it any name and location that you want.

![alt text](/img/consoleapplication.png "Visual Studio New Project dialog")

### Adding NuGet Packages

Both RogueSharp and RLNET are available via NuGet and are easy to add to our project.

1. **Right click** on the project (the name of your project will be whatever you named it in step 3 above)
1. Choose **Manage NuGet Packages…** from the context menu.

![alt text](/img/managenugetpackages.png "Manage NuGet Packages modal")

Now you should be able to choose **Online -> nuget.org** and search for **RogueSharp** and **RLNET**. Click **Install** for both packages. As of this writing the latest version of RogueSharp is 2.0.0.0

![alt text](/img/addingnugetpackages.png "Adding NuGet Packages dialog")

#### Adding a Font File

RLNET needs a special font file like libtcod uses. If you aren’t familiar with this and want to know more about it there is [good information here](http://doryen.eptalys.net/data/libtcod/doc/1.5.1/html2/console_set_custom_font.html?py=true).

But for our purposes just right click and download this image and place it in your project directory.

![alt text](/img/terminal8x8.png "terminal fonts")

Once the file is in your project directory, you should be able to **Right Click** on your project in Visual Studio and choose **Add -> Existing Item…** Choose the image that you saved and it should show up in the project. Make sure to set the **Properties** of this file to **Copy always**.

![alt text](/img/bitmapfont.png "Setting copy always properties in Visual Studio")

### The Code

Because I’ve already produced a bunch of tutorials for RogueSharp and RLNET has it’s own set of tutorials I’m not going to explain each step. The code is only about 100 lines with comments and extra spacing and hopefully my comments are sufficient to help you out. Just copy and paste the code block below into **Program.cs**

* [RogueSharp MonoGame Tutorials]()
* [RLNET Tutorials](https://clarktravism.wordpress.com/)

{% highlight csharp %}
using RLNET;
using RogueSharp;

namespace RogueSharpRLNetSamples
{
  public class Program
  {
    // The screen height and width are in number of tiles
    private static readonly int _screenWidth = 50;
    private static readonly int _screenHeight = 50;
    // The starting position for the player
    private static int _playerX = 25;
    private static int _playerY = 25;
    private static RLRootConsole _rootConsole;
    private static IMap _map;

    public static void Main()
    {
      // Use RogueSharp to create a new cave map the same size as the screen.
      _map = Map.Create( new CaveMapCreationStrategy<Map>( _screenWidth, _screenHeight, 45, 4, 3 ) );
      // This must be the exact name of the bitmap font file we are using or it will error.
      string fontFileName = "terminal8x8.png";
      // The title will appear at the top of the console window
      string consoleTitle = "RougeSharp RLNet Tutorial" ;
      // Tell RLNet to use the bitmap font that we specified and that each tile is 8 x 8 pixels
      _rootConsole = new RLRootConsole( fontFileName, _screenWidth, _screenHeight, 8, 8, 1f, consoleTitle );
      // Set up a handler for RLNET's Update event
      _rootConsole.Update += OnRootConsoleUpdate;
      // Set up a handler for RLNET's Render event
      _rootConsole.Render += OnRootConsoleRender;
      // Begin RLNET's game loop
      _rootConsole.Run();
    }

    // Event handler for RLNET's Update event
    private static void OnRootConsoleUpdate( object sender, UpdateEventArgs e )
    {
      RLKeyPress keyPress = _rootConsole.Keyboard.GetKeyPress();
      if ( keyPress != null )
      {
        // Check for the Up key press
        if ( keyPress.Key == RLKey.Up )
        {
          // Check the RogueSharp map to make sure the Cell is walkable before moving
          if ( _map.GetCell( _playerX, _playerY - 1 ).IsWalkable )
          {
            // Update the player position
            _playerY--;
          }
        }
        // Repeat for the other directions
        else if ( keyPress.Key == RLKey.Down )
        {
          if ( _map.GetCell( _playerX, _playerY + 1 ).IsWalkable )
          {
            _playerY++;
          }
        }
        else if ( keyPress.Key == RLKey.Left )
        {
          if ( _map.GetCell( _playerX - 1, _playerY ).IsWalkable )
          {
            _playerX--;
          }
        }
        else if ( keyPress.Key == RLKey.Right )
        {
          if ( _map.GetCell( _playerX + 1, _playerY ).IsWalkable )
          {
            _playerX++;
          }
        }
      }
    }

    // Event handler for RLNET's Render event
    private static void OnRootConsoleRender( object sender, UpdateEventArgs e )
    {
      _rootConsole.Clear();

      // Use RogueSharp to calculate the current field-of-view for the player
      _map.ComputeFov( _playerX, _playerY, 50, true );

      foreach ( var cell in _map.GetAllCells() )
      {
        // When a Cell is in the field-of-view set it to a brighter color
        if ( cell.IsInFov )
        {
          _map.SetCellProperties( cell.X, cell.Y, cell.IsTransparent, cell.IsWalkable, true );
          if ( cell.IsWalkable )
          {
            _rootConsole.Set( cell.X, cell.Y, RLColor.Gray, null, '.' );
          }
          else
          {
            _rootConsole.Set( cell.X, cell.Y, RLColor.LightGray, null, '#' );
          }
        }
        // If the Cell is not in the field-of-view but has been explored set it darker
        else if ( cell.IsExplored )
        {
          if ( cell.IsWalkable )
          {
            _rootConsole.Set( cell.X, cell.Y, new RLColor( 30, 30, 30 ), null, '.' );
          }
          else
          {
            _rootConsole.Set( cell.X, cell.Y, RLColor.Gray, null, '#' );
          }
        }
      }

      // Set the player's symbol after the map symbol to make sure it is draw
      _rootConsole.Set( _playerX, _playerY, RLColor.LightGreen, null, '@' );

      // Tell RLNET to draw the console that we set
      _rootConsole.Draw();
    }
  }
}
{% endhighlight %}

If you run your project now you should see something like this:

![alt text](/img/rlnetcave.gif "Animation of running tutorial")

[Here is the link to the final code on BitBucket](https://bitbucket.org/FaronBracy/roguesharprlnetsamples)

### Additional Fun

[The eleventh Seven Day Roguelike Challenge is nearly upon us!](http://7drl.org/2015/01/28/7drl-2015-announced-for-7-15-march/) I urge you to participate. If you don’t like RogueSharp or C# there are plenty of other RogueLike libraries out there for most languages.

There are also more .NET console libraries that should work well with RougeSharp. Why don’t you give them a try, or go further with RLNET. If you haven’t used C# before maybe try it for your [7DRL](http://7drl.org/) this year.

* [SadConsole](https://github.com/Thraka/SadConsole) – Works with MonoGame! I haven’t tried it yet but want to soon.
* [Malison](https://bitbucket.org/munificent/malison/wiki/Home) – I have not tried this one yet either but it looks promising.
* [RLNET](https://bitbucket.org/clarktravism/rlnet) – What we used in this tutorial.
