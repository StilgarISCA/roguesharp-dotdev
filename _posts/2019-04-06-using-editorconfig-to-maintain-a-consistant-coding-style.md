---
layout: post
title: "Using EditorConfig to Maintain a Consistent Coding Style"
category:
---
Have you ever tried to work on someone else’s code and realized that they have completely different code formatting than you are used to? Maybe they use tabs, 2 spaces, or 4 spaces for indentation. Maybe they use underscore prefixed private member variables such as `private string _name;`

Wouldn’t it be nice if when you pulled down the project from source control, it also brought all of the code formatting styles with it? That’s what [EditorConfig](https://editorconfig.org/) is meant for.

Not long ago I added an `.editorconfig` file to [RogueSharp](https://github.com/FaronBracy/RogueSharp). I recently mapped all of the ReSharper settings to the config as both Visual Studio and ReSharper come bundled with native support for EditorConfig.

### Download

First you’ll need to download the file. I created a public Github Gist which is available here:

* https://gist.github.com/FaronBracy/155d8d7ad98b4ceeb526b9f47543db1b

### Add to Solution

* Copy the downloaded file to the root of your solution where the .sln file lives.
* Make sure that the name is `.editorconfig`
* Create a new Solution Items folder in Visual Studio if it doesn’t already exist
* Add `.editorconfig` to the Solution Items folder

![alt text](/img/editorconfig_solution.png "Add .editorconfig to Solution Items folder")

### Modify Settings

Although the `.editorconfig` will work as is, you’ll likely want to make some modifications to it, to match your coding style.

### Common Settings

* tabs vs spaces
* indent size
* trim trailing whitespace

![alt text](/img/editorconfig_commonsettings.png "sample settings")

You’ll likely want to change the indent size at a minimum. At [TechSmith](https://www.techsmith.com/) where I am currently employed we use 3 spaces, but 2 or 4 is more typical.

### .NET Language Conventions

![alt text](/img/editorconfig_dotnetnamingconventions.png ".NET naming settings")

[Microsoft Documentation For .NET Language Conventions](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference?view=vs-2019#language-conventions)

### .NET Formatting Conventions

![alt text](/img/editorconfig_dotnetformattingconventions.png ".NET formatting settings")

[Microsoft Documentation For .NET Formatting Conventions](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference#formatting-conventions)

### .NET Naming Conventions

![alt text](/img/editorconfig_dotnetnamingconventions.png ".NET naming settings")

[Microsoft Documentation For .NET Naming Conventions](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-naming-conventions)

### ReSharper Settings

![alt text](/img/editorconfig_resharpersettings.png "Resharper settings")

[JetBrains Documentation For ReSharper EditorConfig Settings](https://www.jetbrains.com/help/resharper/EditorConfig_Index.html)

Note: I organized the ReSharper settings according to how they appear in the menus. There should be a 1-1 relationship between each `.editorconfig` setting and it’s corresponding ReSharper menu entry.

![alt text](/img/editorconfig_resharpermenus.png "How the settings file maps to Resharper in-app formatting settings")

### About Faron

Faron Bracy is a husband, father, and TechSmith employee. He maintains the open source library [RogueSharp](https://github.com/FaronBracy/RogueSharp) as a hobby project. Other hobbies include painting fantasy miniatures, playing board games, and playing retro video games with his family.