---
title:  "#1 My first Post!"
date:   2018-04-04 00:40:01 +0200
tags: ITLengine worklog 
layout: default
comments: true
---

Hello World. This actually the first post I ever wrote seen in the public. That's creepy. Well maybe not ... we will see. Phew the first few words I ever wrote in a blog are done. :D

# Intentions #
As I got my Engine the first time running and doing that what it was thought to do, I started thinking how to document the progress and how to do the homework for my english class next week. The Solution: This. You may notice my english needs a little more polish than my code and it's possible that I am changing these articles more often to get the style (word-wise and style-wise) right, but it is a beginning.

# What I did? #
Some of you may know I have the ["Is there Life? Point and Click Adventure Engine" (ITLengine)-Project](https://github.com/HaSa1002/Is-there-Life-Point-and-Click-Adventure-Engine) since October last year. I startet making some menues and it fast became visibly the state we have today (or better yesterday). Today I was working at the scripting, as the whole process extremly slowed down to zero and I got angry. To explain that I have to start earlier: The time I started the engine I did everything in the ImGui-Menues and in two classes, perhaps three to five... I came across a [game development article by Optank](http://optank.org/game-development-design/) - one of the collaborators and moderators of [SFML](https://sfml-dev.org). It was about the so called [Message Bus System](http://optank.org/2013/04/02/game-development-design-3-message-bus/), which is a system to minimize dependencies through letting every object communicating through this "Bus". I liked the idea and started implementing it, while finding out that it was harder then I thought, but 20 hours later it worked... coming with one real drawback: It made everything more complicated and complex. I never really used it nicely but always kept an eye on not loosing the possibility of using it. So said making things slowed down. About some days later I came across the [entity component system](http://optank.org/2012/11/14/game-development-design-1-the-component-system/). I had the bad idea of implementing one. It took about 3 monthes and it isn't really finished even today. I had this untested solution not knowing how to use or wire into the engine. Around this time I came across [Lua](https://lua.org). Lua is a nice and powerfull scripting language. As I read [Elias Daler Post](https://eliasdaler.github.io/game-object-references/) about Lua and his ways of doing this I saw how to make a nice scripting API, but the real essence I got from him was a magic word or better a magic project: [Solv2](https://github.com/ThePhD/sol2). Since Lua's API is completly written in C it has no classes or other things that make C++ great. But Solv2 creates an interface on top of that in C++, which is very clear and straight-foreward to use. I tested a little and then we are at the point I mentioned earlier: Nothing worked, no process was seen and I got angry and mad about those facts. I wrote a little with people in the [community around Andreas Suika](https://discord.gg/acht) (Creative Director [The Long Jouney Home](http://tljhgame.com/)) better known as ["Acht"](https://discord.gg/acht) (german for eight - don't try to understand the insider: Just a few of us are knowing the reason). They gave me two tipps: Tea and Refactoring. The first is shorter: I made darjeeling black tea (from the [teacampaign](https://www.teacampaign.com/)) and it helped magically thinking positive of the second tipp:

## Refactoring ##
The problem with the halfway integrated systems in my old engine was, that they required the same things to do to get anything working. Also I had this stupid idea of an engine mainmenue which is two months older than the engine. It was a time to do something I do more ofen but also often results in dead projects: Creating a new project while abondining all the old stuff I did. Well that's not completly right this time, as I copied six files, but not more! From 20 files to six: That's great but what is even better? I got rid of my entity component system and the message bus. I than fastly wrote the new scripting functions and changed a little in my rendering and the engine class and the only five hours later: It worked. The object I wanted to achieve got - in a very basical way - achieved.

## The achievement ##
I now create a file where I script my scene using some tables and some callback functions. I write tomorrow about that in detail. Once loaded, the object(s) get rendered and I get feedback when clicking on it. I even created a localisation system only in lua with just creating two additional tables and one function which reads the key and returns the string. As said, tomorrow the how.

# About this post #
Before I write the post for tomorrow, I am going to add some links to this and re-read this one. Until then: Have fun reporting mistakes. :D

# End quote #
>Making something small big is easier then making something big small.
>
>-- HaSa
