---
layout: post
title: "Collecting Code Coverage Metrics for .NET Core Libraries in Azure DevOps"
category:
---
Last time I talked about how I added status badges to the Readme file for [RogueSharp](https://github.com/FaronBracy/RogueSharp). One of the badges was for showing the Code Coverage results from my continuous Azure DevOps build.

Before you can have a badge display those numbers, you need to have a build that can actually generate and collect them. While searching I found a lot of information about how to do it for full .NET Framework projects but information on .NET Core was scarce.

Since it wasn’t very straightforward for me I’ll share what I did here. Maybe there is a better way now?

### MSTest Version

The first issue I struggled with was realizing that the version of MSTest that was I was using did not support the option to collect code coverage.

https://www.nuget.org/packages/MSTest.TestFramework/

You’ll need at least version 1.4.0.

Here is the commit where I upped my version, so up yours!

https://github.com/FaronBracy/RogueSharp/commit/10775b73baaec0785155ad2be287a5c255d0212b

![alt text](/img/mstest_version.png "MS Test NuGet versions for code coverage")

### Azure DevOps Continuous Build

The Azure DevOps continuous build should then be using the .NET Core task.


![alt text](/img/net_core_task.png "The .NET Core build task in Azure DevOps")

I added three distinct steps.

1. Restore NuGet Packages
1. Build
1. Run Tests

![alt text](/img/build_steps.png "Restore, Build, Test Build Steps")

It’s this third step for running tests that will get us code coverage.


![alt text](/img/test_step_setup.png "Checkbox for Publish test results and code coverage)

* Under Arguments check the box next to Publish test results and code coverage
* The Arguments should be `--configuration $(BuildConfiguration) --collect "Code coverage"`

The `--collect "Code coverage"` bit will only work if you are on MSTest version 1.4.0 or higher. It also appears to be case sensitive so make sure you type it exactly as shown.

### Viewing Code Coverage Metrics

Once this is setup trigger the continuous build. If you look in the summary you should see the code coverage showing up now.


![alt text](/img/build_summary.png "Azure DevOps build summary showing code coverage")

[View Build Results In Azure DevOps](https://dreamersdesign.visualstudio.com/RogueSharp/_build/results?buildId=28&view=results)

### Code Coverage Badge

![alt text](/img/1.svg "Build coverage badge")

[Shields.IO](https://shields.io/category/coverage) supports code coverage badges for multiple build services.

The formatted URL for Azure DevOps looks like this:

https://img.shields.io/azure-devops/coverage/{ORGANIZATION}/{PROJECT}/{DEFINITION_ID}.svg

See my previous post on status badges/shields for more information

### About Faron

Faron Bracy is a husband, father, and TechSmith employee. He maintains the open source library [RogueSharp](https://github.com/FaronBracy/RogueSharp) as a hobby project. Other hobbies include painting fantasy miniatures, playing board games, and playing retro video games with his family.