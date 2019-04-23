---
layout: post
title: "RogueSharp 2.0 Released"
tagline: "Dice Notation"
category: release-notes
---

## RogueSharp Version 2.0

on 2.0 of RogueSharp was just released. It now contains better functionality for dealing with Dice and parsing [Dice Notation](http://en.wikipedia.org/wiki/Dice_notation) strings.

* [RogueSharp v2.0 Nuget Package](https://www.nuget.org/packages/RogueSharp/2.0)
* [RogueSharp v2.0 Source Code](https://github.com/FaronBracy/RogueSharp/releases/tag/v2.0)

[Dice Notation .NET](https://dicenotation.codeplex.com/) – Significant portions of the code in RougeSharp.DiceNotation namespace are from this great library. Thank you to Chris Wagner for originally creating this.

* [Dice Notation .NET source code](https://dicenotation.codeplex.com/SourceControl/latest)
* [Dice Notation .NET license](https://dicenotation.codeplex.com/license)

### Usage

The easiest way to roll dice using [dice notation](http://en.wikipedia.org/wiki/Dice_notation) is to use the static Roll method on the Dice class and provide a string representing the dice expression.

**Example 1**

{% highlight csharp %}
// Roll 3 dice with 6 sides each and sum the results together
// This produces a result between 3 and 18
int result = Dice.Roll( "3d6" );
{% endhighlight %}

**Example 2**

{% highlight csharp %}
// Roll 4 dice with 6 sides each
// But then only keep the 3 highest rolls
// This produces a result between 3 and 18
// Higher results should be produced on average than rolling 3d6
int result = Dice.Roll( "4d6k3" );
{% endhighlight %}

**Example 3**

{% highlight csharp %}
// Roll 1 die with 10 sides and add 5 to the result
// This produces a result between 6 and 15
int result = Dice.Roll( "1d10+5" );
{% endhighlight %}

**Example 4**

{% highlight csharp %}
// Roll 1 die with 10 sides and 2 dice with 6 sides each...
// Sum up the values of the dice and subtract 5 from the final result
// This produces a result between -2 and 17
int result = Dice.Roll( "1d10+2d6-5" );
{% endhighlight %}

### Creating Expressions Fluently

If you haven’t heard of Fluent interfaces you can check out the link on Wikipedia to get a better description than I could provide. I think of it is using method chaining to produce more readable code. If you have ever used LINQ and seen how you could chain methods together to build your queries, I think that is a decent example. Let’s see how you can build a DiceExpression fluently

{% highlight csharp %}
// In dice notation the following is the same as "5+1d8+4d6k3"
DiceExpression diceExpression = new DiceExpression()
  .Constant( 5 ) // Add a constant of 5
  .Die( 8 )      // Add an 8 sided Die
  .Dice( 4, 6, choose: 3 ); // Add 4d6 but when rolling them keep the 3 highest results

// Total should be between 9 and 31.
DiceResult result = diceExpression.Roll();
int total = result.Value;
{% endhighlight %}

### Upgrading from v1.X

If you are were using an earlier version of RogueSharp and you used the `Dice` or `Die` classes there will be some minor changes that you will have to make to use v2.0. I have upgraded the sample for the tutorial series here so you can get an idea of what it will take: https://bitbucket.org/FaronBracy/roguesharpmonogamesamples/commits/25a57cc9820bcb2e75a043d76ca6bf32020db2ff