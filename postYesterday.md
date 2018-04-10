----
title: #3 new Lua structure and the Future
date: ...
comments: true
tags: Lua, C++, ITLengine, worklog, personal, blog
----

# Content #
* Lua
** Changes in the scene structure
** New functions for interactions between Game and Engine
* New Engine Features
** A big step to the scene editor
** Changing the Scene - The Implementation
** Making the game talkier: Subtitles
* The Future
** Engine
** Blog
** Twitch
* End Quote

# Lua #
## Changes in the scene structure ##
As I started adding the Callbacks I created non-sense like this:


[CODE OF THE CALLBACK 1. VERSION] and [Lua Callback back then]


As you see I first check for an action to call and to call the callback with the action so the callback itself checks again for the action. It’s so stupid as this sentence and so it was changed. As you may know, in Lua only tables as container exist, and those tables can have functions, and any kind of variables as Lua is loose typed. Now, we have those tables and every object is a new table, that said we just add our functions to the table of the objects like this:


[Code of a new object with callback]


I think that’s a nice short way of initializing the objects and callbacks. You may noticed the old callback parameter got erased because we have the object named and the as the functions belong to the object we just need to set their name in the structure onAction().

## New functions for interactions between Game and Engine
As the part above became functional I needed a bit prove that they were called, so added two Lua functions for changing the scene and setting the subtitle. Their power will get demonstrated later, since the actual implementation was in the Engine and not in Lua. For the protocol: the Lua functions are just setting a variable in the game table.

# New Engine Features #
## A big step to the scene editor
It is now possible to “pick-up” an object and move it around. To do that you have to be in editor mode, which is always accessible by pressing F3 and then hover the object, you want to pickup press P and move your mice. Once you are satisfied with your achievement you can press P again and it will be dropped. The same applies for other Objects which have a red border around: You can move them.

[Gif which shows the editor mode and moving]

## Changing the scene - The Implementation ##
Since I am aware of calling C Functions from Lua directly, in my opinion it is not necessary, and it might kill your performance. What my Lua functions all do is just setting a specific variable, in this case the games.loadScene. The variable is read and reset by the engine when it’s the time to: most often after a click event or a hovering. It could – theoretically – any SFML Event, since I am using SFML and handling those events. I then reset the scene and set the new name of the scene, afterwards I give this empty package, with just a name to my Lua Handling and tell: Load. My loading stayed basically the same.

## Making the game talkier: Subtitles ##
I told about Subtitles and here they are: They are so important, because often we don’t have the resources to record the audio and even then, I personally prefer playing with subtitles, that they got an own class. Well that’s not the only reason as I perhaps can implement a lot, and with a lot I mean a lot, of features you can use to make them nicer, but that wasn’t my approach. That was the approach of the old engine: “I perhaps eventually could need that sometime in the future, so implement it”, my new approach is YAGNI. Whom? YAGNI stands for “You aint gonna need it”, and if you implement that way the progress you make is faster then you. Spoken sketchy… Well, back to the topic: Subtitles. By now they can be configured in size, color and font. The best is, that the are always positioned in the center of the window. The real center, not just at the beginning of the text. Here a prove that I am not lying: 
![Prove]()
And even better:
![prove multiline]()
It also works with multiple lines. I added special characters support to get support for languages that are not English. 
![Subtitle with many special characters]()

# The Future #
## Engine ##
As the engine now gets shape I think we will start producing our game soon. With that we are going to add new features and get the engine a bit more polished. Since this is an open source project I’d welcome any help I can get. Especially for finding some good tutorials and articles about CMake. :D

## Blog ##
The last three articles (including this one) were written in my Easter holidays. Since they are over tomorrow I won’t have time to bring articles this often. I like the idea of getting into rhythm to add this to my schedule and collect a bit about what I will write. I don’t think that posting every week would work so I’d like to post every second Monday starting now, what means we have the next post, because this counts for yesterday, at the 23rd April. If I get something done in the meantime I probably won’t wait until the next Monday. We will see.

## Twitch ##
If you are interested in development streams, then you may want to visit my Twitch Channel. I stream irregularly and spontaneous. Plus, I am starting often in German and English and switch to language preferred by my watchers.
