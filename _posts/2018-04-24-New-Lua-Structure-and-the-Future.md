---
title: "#3 New Lua Structure and the Future"
date: 2018-04-24 21:00:00 +0200
comments: true
tags: Lua C++ ITLengine worklog personal blog
layout: default
comments: true
---

# Content #
- Lua
	- Changes in the scene structure
	- New functions for interactions between Game and Engine
- New Engine Features
	- A big step to the scene editor
	- Changing the Scene - The Implementation
	- Making the game talkier: Subtitles
- The Future
	- Engine
	- Blog
	- Twitch
- End Quote

### Images missing. Sorry :( ###

# Lua #
## Changes in the scene structure ##
As I started adding the Callbacks I created non-sense like this:


<pre class="vs-code">scene[<span class="string">&quot;objects&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
<span class="comment">--&nbsp;&nbsp;{Name&nbsp;of&nbsp;the&nbsp;Object,&nbsp;Position:&nbsp;x,y,z,&nbsp;Texturepath,&nbsp;actions(use&nbsp;|&nbsp;collect,&nbsp;look,&nbsp;hover)}
</span>&nbsp;&nbsp;&nbsp;&nbsp;{<span class="string">&quot;Panel&quot;</span>,&nbsp;<span class="number">400</span>,&nbsp;&nbsp;<span class="number">389</span>,&nbsp;<span class="number">1</span>,&nbsp;<span class="string">&quot;.\\textures\\test2.png&quot;</span>,&nbsp;{<span class="string">&#39;u&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>,&nbsp;<span class="string">&#39;h&#39;</span>}}
&nbsp;&nbsp;&nbsp;&nbsp;{<span class="string">&quot;OtherRoom&quot;</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="number">1</span>,&nbsp;<span class="string">&quot;r100,900&quot;</span>,&nbsp;{<span class="string">&#39;u&#39;</span>,&nbsp;<span class="string">&#39;h&#39;</span>}}
&nbsp;&nbsp;&nbsp;&nbsp;{<span class="string">&quot;TextDemo&quot;</span>,&nbsp;<span class="number">790</span>,&nbsp;<span class="number">500</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="string">&quot;tI&nbsp;am&nbsp;a&nbsp;text!&quot;</span>,&nbsp;{<span class="string">&#39;l&#39;</span>,&nbsp;<span class="string">&#39;h&#39;</span>}}
}
<span class="keyword">function</span>&nbsp;<span class="type">scene.onPanelUse</span><span class="operator">()</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="keyword">setSubtitle</span>(<span class="keyword">display</span>(<span class="string">&quot;PanelUse&quot;</span>))
<span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.onPanelLook</span><span class="operator">()</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="keyword">setSubtitle</span>(<span class="keyword">display</span>(<span class="string">&quot;PanelLook&quot;</span>))
<span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.onPanelHover</span><span class="operator">()</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="keyword">setSubtitle</span>(<span class="keyword">display</span>(<span class="string">&quot;PanelHover&quot;</span>))
<span class="keyword">end</span></pre>


As you see I first check for an action to call and to call the callback with the action so the callback itself checks again for the action. It’s so stupid as this sentence and so it was changed. As you may know, in Lua only tables as container exist, and those tables can have functions, and any kind of variables as Lua is loose typed. Now, we have those tables and every object is a new table, that said we just add our functions to the table of the objects like this:


<pre class="vs-code">scenes[<span class="string">&quot;bedroom_old&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{}
<span class="keyword">local</span>&nbsp;scene&nbsp;<span class="operator">=</span>&nbsp;scenes[<span class="string">&quot;bedroom_old&quot;</span>]
scene[<span class="string">&quot;objects&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
	Accesspoint&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">1083</span>,&nbsp;<span class="number">470</span>,&nbsp;<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\Accesspoint.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>,&nbsp;<span class="string">&#39;u&#39;</span>}},
	Sirene&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">904</span>,<span class="number">92</span>,<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\Sirene.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}},
	BettwaescheR1&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">1241</span>,<span class="number">638</span>,<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\Bettwaeschemirrored.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}},
	LifePodDoor&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">572</span>,<span class="number">764</span>,<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\LifePodBoden.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>,&nbsp;<span class="string">&#39;u&#39;</span>}},
	BettwaescheL1&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">294</span>,<span class="number">629</span>,<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\Bettwaesche.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}},
	Door&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">775</span>,<span class="number">317</span>,<span class="number">2</span>,&nbsp;<span class="string">&quot;.\\textures\\Tuer.png&quot;</span>,&nbsp;{<span class="string">&#39;h&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>,&nbsp;<span class="string">&#39;u&#39;</span>}},
	bg2&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">0</span>,<span class="number">0</span>,<span class="number">1</span>,&nbsp;<span class="string">&quot;.\\textures\\HintergrundSchlafraum2.png&quot;</span>},
	bg1&nbsp;<span class="operator">=</span>&nbsp;{<span class="number">0</span>,<span class="number">0</span>,<span class="number">0</span>,&nbsp;<span class="string">&quot;.\\textures\\HintergrundSchlafraum.png&quot;</span>},
}
&nbsp;scene[<span class="string">&quot;walkboxes&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
}
&nbsp;scene[<span class="string">&quot;zoomlines&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
}
 
 
<span class="comment">--&nbsp;Callbacks
</span>
<span class="keyword">function</span>&nbsp;<span class="type">scene.onEnter</span><span class="operator">()</span>
	
<span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.objects.Accesspoint.onHover</span><span class="operator">()</span>
 
<span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.objects.Accesspoint.onLook</span><span class="operator">()</span>
	
<span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.objects.Accesspoint.onUse</span><span class="operator">()</span>
	
<span class="keyword">end</span>
 
<span class="keyword">...</span></pre>


I think that’s a nice short way of initializing the objects and callbacks. You may noticed the old callback parameter got erased because we have the object named and the as the functions belong to the object we just need to set their name in the structure onAction().

## New functions for interactions between Game and Engine
As the part above became functional I needed a bit prove that they were called, so added two Lua functions for changing the scene and setting the subtitle. Their power will get demonstrated later, since the actual implementation was in the Engine and not in Lua. For the protocol: the Lua functions are just setting a variable in the game table.

# New Engine Features #
## A big step to the scene editor
It is now possible to “pick-up” an object and move it around. To do that you have to be in editor mode, which is always accessible by pressing F3 and then hover the object, you want to pickup press P and move your mice. Once you are satisfied with your achievement you can press P again and it will be dropped. The same applies for other Objects which have a red border around: You can move them.

![Gif which shows the editor mode and moving]()

## Changing the scene - The Implementation ##
Since I am aware of calling C Functions from Lua directly, in my opinion it is not necessary, and it might kill your performance. What my Lua functions all do is just setting a specific variable, in this case the games.loadScene. The variable is read and reset by the engine when it’s the time to: most often after a click event or a hovering. It could – theoretically – any SFML Event, since I am using SFML and handling those events. I then reset the scene and set the new name of the scene, afterwards I give this empty package, with just a name to my Lua Handling and tell: Load. My loading stayed basically the same.

## Making the game talkier: Subtitles ##
I told about Subtitles and here they are: They are so important, because often we don’t have the resources to record the audio and even then, I personally prefer playing with subtitles, that they got an own class. Well that’s not the only reason as I perhaps can implement a lot, and with a lot I mean a lot, of features you can use to make them nicer, but that wasn’t my approach. That was the approach of the old engine: “I perhaps eventually could need that sometime in the future, so implement it”, my new approach is YAGNI. Whom? YAGNI stands for “You aint gonna need it”, and if you implement that way the progress you make is faster then you. Spoken sketchy… Well, back to the topic: Subtitles. By now they can be configured in size, color and font. The best is, that the are always positioned in the center of the window. The real center, not just at the beginning of the text. Here a prove that I am not lying: 
![Prove]({{"/assets/3-subtitle.png" | absolute_url}})
And even better:
![prove multiline]({{"/assets/3-multiline.png" | absolute_url}})
It also works with multiple lines. I added special characters support to get support for languages that are not English. 
![Subtitle with many special characters]({{"/assets/3-utfchars.png" | absolute_url}})

# The Future #
## Engine ##
As the engine now gets shape I think we will start producing our game soon. With that we are going to add new features and get the engine a bit more polished. Since this is an open source project I’d welcome any help I can get. Especially for finding some good tutorials and articles about CMake. :D

## Blog ##
The last three articles (including this one) were written in my Easter holidays. Since they wew over two weeks ago I won’t have time to bring articles this often. I like the idea of getting into rhythm to add this to my schedule and collect a bit about what I will write. I don’t think that posting every week would work so I’d like to post third or fourth Monday starting now, what means we have the next post, because this counts for yesterday, at the 14th May. If I get something done in the meantime I probably won’t wait until the next Monday. We will see.

## Twitch ##
If you are interested in development streams, then you may want to visit my [Twitch Channel](https://www.twitch.tv/realhasa). I stream irregularly and spontaneous. Plus, I am starting often in German and English and switch to language preferred by my watchers.
