---
layout: post
title: "Creating Status Badges and Shields for your Repository"
category:
---
I recently added status badges to the Readme file for the open source project I maintain, RogueSharp. I’ve seen other repositories do this but I wasn’t sure how. After a lot of looking around I compiled a list of services that worked for me. I couldn’t seem to find all this information in one place, so I’m hoping that this post showcasing what worked for me will help others.

### Shields.IO

[Shields.IO](https://shields.io) is a service for creating concise, consistent, and legible badges in SVG format. This should be your first stop when looking for a particular badge.

#### Resource Links

![alt text](/img/tutorials-blog-blue.svg "Tutorials blog badge") ![alt text](/img/docs-github-blue.svg "Docs GitHub badge") ![alt text](/img/license-MIT-blue.svg "MIT license badge")

I use [Shields.IO](https://shields.io) to create badges that link to static resources. To create these you just need a specially formatted URL.

{% highlight yaml %}
https://img.shields.io/badge/LABEL-MESSAGE-COLOR.svg
{% endhighlight %}

To create a blue badge for a BSD 3-Clause license we could use

{% highlight yaml %}
https://img.shields.io/badge/license-BSD%203--Clause-blue.svg
{% endhighlight %}

Because our license has a space and a dash in it we will need to treat those specially. The space will need to be replaced with it’s equivalent URL encoding **“%20”** and the dash will need to be escaped with a preceding dash.

Finally we need to decorate it with the appropriate markdown to get it to display as an image which links to the license.

{% highlight markdown %}
[![License: BSD 3-Clause](https://img.shields.io/badge/license-BSD%203--Clause-blue.svg)](https://github.com/TechSmith/hyde/blob/master/LICENSE.txt)
{% endhighlight %}

![alt text](/img/license-BSD-3--Clause-blue.svg "BSD-3 License badge")

#### Test Status

![alt text](/img/1-1.svg "Count of passing tests badge")

It’s likely that no matter which service you are using for running continuous builds, [Shields.IO](https://shields.io) also has you covered.

The formatted URL to get a test status badge for an Azure DevOps pipeline project will look like this:

{% highlight yaml %}
https://img.shields.io/azure-devops/tests/ORGANIZATION/PROJECT/DEFINITION_ID.svg
{% endhighlight %}

When I did this for my own repo I wasn’t quite sure how to fill out **“ORGANIZATION”**, **“PROJECT”** and **“DEFINITION_ID”** placeholders.

What I found is that you can pull these directly from the query string when going to Azure DevOps and viewing your continuous build. In my case the URL is:

{% highlight yaml %}
https://dreamersdesign.visualstudio.com/RogueSharp/_build?definitionId=1
{% endhighlight %}

In this example **“dreamersdesign”** is the organization, **“RogueSharp”** is the project and **“1”** is the definition ID.

#### Code Coverage

![alt text](/img/1.svg "Test coverage badge")

[Shields.IO](https://shields.io) also supports code coverage badges for multiple build services.

The formatted URL for Azure DevOps looks like this:

{% highlight yaml %}
https://img.shields.io/azure-devops/coverage/ORGANIZATION/PROJECT/DEFINITION_ID.svg
{% endhighlight %}

You’ll notice it looks like the test status badge, only replacing **“tests”** with **“coverage”**

### BuildStats.info

Even though [Shields.IO](https://shields.io) supported most of what I needed it didn’t support everything. It looks like development keeps happening on the service, so perhaps that will have changed by the time you read this. For my remaining badges I turned to [BuildStats.info](https://buildstats.info/).

[BuildStats.info](https://buildstats.info/) creates SVG widgets to display build history charts and NuGet badges.

#### NuGet

![alt text](/img/roguesharp.svg "NuGet successful build badge")

This one couldn’t be simpler. Just create a URL to

{% highlight yaml %}
https://buildstats.info/nuget/PACKAGE_NAME
{% endhighlight %}

So for RogueSharp this was just

{% highlight yaml %}
https://buildstats.info/nuget/roguesharp
{% endhighlight %}

It will automatically pull in the latest version number as well as the number of downloads.

#### Build History Chart

![alt text](/img/roguesharp-20n28.svg "Build history chart")

At the time that I setup my badges, [BuildStats.info](https://buildstats.info/) didn’t support the Build History Chart for Azure DevOps. It does look like this is now supported, but for my chart I used AppVeyor.

The formatted URL looks like this:

{% highlight yaml %}
https://buildstats.info/BUILD_SYSTEM/chart/ACCOUNT/PROJECT
{% endhighlight %}

Substituting the values for my repository this yielded:

{% highlight yaml %}
https://buildstats.info/appveyor/chart/FaronBracy/roguesharp-20n28
{% endhighlight %}

### Build Service Specific

It’s worth noting that many build services provide their own status badge links. They probably won’t provide you with anything that you can’t get with Shields.IO, but it may be worth looking into.

#### Azure DevOps

![alt text](/img/RogueSharp_Continuous.svg "Azure Pipelines build succeeded badge")

When you are looking at your build pipeline you’ll see an elipsis for the build… click that.

![alt text](/img/azuredevops_menu.png "Azure DevOps edit menu")

Then click on Status badge

![alt text](/img/azuredevops_badges.png "Azure DevOps status badge screen")

This will give you markdown that you can copy and paste directly into your readme.

#### AppVeyor

![alt text](/img/mx09mla59wsgrkkj.svg "AppVeyor passing build badge")

Click on AppVeyor configuration for a repo (the gear icon). Then select **“Badges”** from the tabs on the left.

![alt text](/img/appveyor_badges.png "AppVeyor configuration screen")

This will give you markdown that you can copy and paste directly into your readme.


### About Faron

Faron Bracy is a husband, father, and TechSmith employee. He maintains the open source library [RogueSharp](https://github.com/FaronBracy/RogueSharp) as a hobby project. Other hobbies include painting fantasy miniatures, playing board games, and playing retro video games with his family.