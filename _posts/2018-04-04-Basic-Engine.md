---
title:  "#2 The Basic Engine"
date:   2018-04-04 18:30:00 +0200
tags: ITLengine worklog lua c++
layout: default
---
As I said yesterday, today is the moment, I show a lot of the engine of today’s build. It will get technical, but if it gets un-understandable tell me.

# How it works – The Lua Part #
We have a little Lua to initialize the engine with all the needed tables. If they aren’t existing, the engine is hopelessly lost.

<pre style="font-family:Consolas;font-size:13;color:black;background:white;">scenes&nbsp;=&nbsp;{}
localisations&nbsp;=&nbsp;{}
game&nbsp;=&nbsp;{}</pre>

As you see we have a scenes table, which obviously holds all the scenes, a localizations table which isn’t needed really in the engine now and finally a game table.
I use the game table for saving configurations of the game, like the language or other game related settings.
Once we initialized the tables we fill them.

<pre style="font-family:Consolas;font-size:13;color:black;background:white;"><span style="color:blue;">dofile</span>&nbsp;<span style="color:#a31515;">&quot;locals/en.lang&quot;</span>
<span style="color:blue;">dofile</span>&nbsp;<span style="color:#a31515;">&quot;locals/de.lang&quot;</span>
 
<span style="color:blue;">dofile</span>&nbsp;<span style="color:#a31515;">&quot;scenes/lua_testscene.pcscene&quot;</span>
 
<span style="color:green;">--&nbsp;Now&nbsp;some&nbsp;game&nbsp;config
</span><span style="color:blue;">dofile</span>&nbsp;<span style="color:#a31515;">&quot;Game.conf&quot;</span>
game.stdlang&nbsp;=&nbsp;<span style="color:#a31515;">&quot;en&quot;</span></pre>

That’s the moment it gets annoying: We must load every single file, and as I am to lazy to write a function, which iterates through a folder, we must add them manually to the file or write them as you wish. All the engine internally does is to execute this single “Engine.lua”. Everything else is in your hand! Let’s see how a scene file looks like:

# How it works – The Scene File #

<pre style="font-family:Consolas;font-size:13;color:black;background:white;">scenes[<span style="color:#a31515;">&quot;lua_testscene&quot;</span>]&nbsp;=&nbsp;{}
<span style="color:green;">--&nbsp;We&nbsp;are&nbsp;lazy
</span><span style="color:blue;">local</span>&nbsp;scene&nbsp;=&nbsp;scenes[<span style="color:#a31515;">&quot;lua_testscene&quot;</span>]
 
<span style="color:green;">--&nbsp;The&nbsp;Objects&nbsp;now:&nbsp;Format&nbsp;{texture&nbsp;path,&nbsp;position:&nbsp;x,y,z,&nbsp;callback&nbsp;(or&nbsp;nil),&nbsp;{actions&nbsp;(ether&nbsp;u&nbsp;or&nbsp;c)&nbsp;and/or&nbsp;l}&nbsp;(or&nbsp;nil)}
</span>scene[<span style="color:#a31515;">&quot;objects&quot;</span>]&nbsp;=&nbsp;{
	{<span style="color:#a31515;">&quot;.\\textures\\test.png&quot;</span>,&nbsp;0,&nbsp;0,&nbsp;0,&nbsp;<span style="color:#a31515;">&quot;bin&quot;</span>,&nbsp;{<span style="color:#a31515;">&#39;u&#39;</span>,&nbsp;<span style="color:#a31515;">&#39;l&#39;</span>}},
	<span style="color:green;">--&nbsp;This&nbsp;one&nbsp;is&nbsp;special:&nbsp;It&nbsp;gets&nbsp;explained&nbsp;later
</span>	{<span style="color:#a31515;">&quot;r800,600&quot;</span>,&nbsp;200,&nbsp;100,&nbsp;1,&nbsp;<span style="color:#a31515;">&quot;hello&quot;</span>,&nbsp;{<span style="color:#a31515;">&#39;u&#39;</span>,&nbsp;<span style="color:#a31515;">&#39;l&#39;</span>}}
}
 
<span style="color:green;">--&nbsp;The&nbsp;Walkboxes:&nbsp;Format:&nbsp;{position:&nbsp;x,y,&nbsp;width,&nbsp;height,&nbsp;isActive&nbsp;(bool)}
</span>scene[<span style="color:#a31515;">&quot;walkboxes&quot;</span>]&nbsp;=&nbsp;{
	{0,&nbsp;10,&nbsp;1000,&nbsp;400,&nbsp;<span style="color:blue;">true</span>},
	{200,&nbsp;10,&nbsp;1000,&nbsp;400,&nbsp;<span style="color:blue;">true</span>},
	{150,&nbsp;300,&nbsp;60,&nbsp;200,&nbsp;<span style="color:blue;">true</span>}
}
 
<span style="color:green;">--&nbsp;The&nbsp;Zoomlines:&nbsp;Format&nbsp;{point1:&nbsp;x,y,&nbsp;point2:&nbsp;x,y,&nbsp;zoomfactor,&nbsp;isActive}
</span>scene[<span style="color:#a31515;">&quot;zoomlines&quot;</span>]&nbsp;=&nbsp;{
	{0,&nbsp;0,&nbsp;1000,&nbsp;1000,&nbsp;0.9,&nbsp;<span style="color:blue;">true</span>}
}
 
<span style="color:blue;">function</span>&nbsp;<span style="color:#2b91af;">scene.hello</span>(&nbsp;action&nbsp;)
	<span style="color:green;">--&nbsp;display&nbsp;hello&nbsp;testy&nbsp;World&nbsp;
</span>	<span style="color:blue;">print</span>(<span style="color:blue;">display</span>(<span style="color:#a31515;">&quot;test&quot;</span>));&nbsp;<span style="color:green;">--&nbsp;test
</span><span style="color:blue;">end</span>
 
<span style="color:blue;">function</span>&nbsp;<span style="color:#2b91af;">scene.bin</span>(&nbsp;action&nbsp;)
	<span style="color:green;">--&nbsp;display&nbsp;depending&nbsp;on&nbsp;action&nbsp;runs&nbsp;or&nbsp;it&#39;s&nbsp;a&nbsp;tablet
</span>	<span style="color:blue;">if</span>&nbsp;action&nbsp;==&nbsp;<span style="color:#a31515;">&#39;u&#39;</span>&nbsp;<span style="color:blue;">then</span>&nbsp;<span style="color:blue;">print</span>(<span style="color:blue;">display</span>(<span style="color:#a31515;">&quot;tabletU&quot;</span>))
	<span style="color:blue;">elseif</span>&nbsp;action&nbsp;==&nbsp;<span style="color:#a31515;">&#39;l&#39;</span>&nbsp;<span style="color:blue;">then</span>&nbsp;<span style="color:blue;">print</span>(<span style="color:blue;">display</span>(<span style="color:#a31515;">&quot;tabletL&quot;</span>))
	<span style="color:blue;">end</span>
<span style="color:blue;">end</span>
 
<span style="color:blue;">return</span>&nbsp;scene</pre>

This simple file results in this simple scene:
![Result]({{"/assets/2-result-scene.png" | absolute_url}})
Even the callbacks are working:
![Callbacks]({{"/assets/2-output.png" | absolute_url}})
Once we finished the loading of the file we are loading this specific scene - this get changed, don’t worry! Now we get to the interesting stuff in the engine:

# How it works – The Static Part #
Once Lua is done my engine gratefully says thanks and adds the objects, walk boxes, and zoom lines. To a scene which it got from the engine manager (better known as Engine).

<pre style="font-family:Consolas;font-size:13;color:black;background:white;"><span style="color:green;">//first&nbsp;is&nbsp;the&nbsp;key,&nbsp;second&nbsp;the&nbsp;value</span>
<span style="color:blue;">void</span>&nbsp;<span style="color:#2b91af;">Lua</span>::readObject(std::<span style="color:#2b91af;">pair</span>&lt;sol::<span style="color:#2b91af;">object</span>,&nbsp;sol::<span style="color:#2b91af;">object</span>&gt;&nbsp;<span style="color:gray;">o</span>)&nbsp;{
	<span style="color:blue;">if</span>&nbsp;(!<span style="color:gray;">o</span>.second.is&lt;sol::<span style="color:#2b91af;">table</span>&gt;())
		<span style="color:blue;">throw</span>&nbsp;<span style="color:#2b91af;">Exception</span>::<span style="color:darkslategray;">noObjectTable</span>;&nbsp;<span style="color:green;">//OOops&nbsp;the&nbsp;Lua&nbsp;was&nbsp;wrong</span>
 
	std::<span style="color:#2b91af;">list</span>&lt;<span style="color:blue;">char</span>&gt;&nbsp;actions;
	<span style="color:green;">//We&nbsp;need&nbsp;to&nbsp;read&nbsp;the&nbsp;actions,&nbsp;because&nbsp;it&#39;s&nbsp;a&nbsp;table</span>
	<span style="color:blue;">auto</span>&nbsp;getActions&nbsp;=&nbsp;[&amp;actions](std::<span style="color:#2b91af;">pair</span>&lt;sol::<span style="color:#2b91af;">object</span>,&nbsp;sol::<span style="color:#2b91af;">object</span>&gt;&nbsp;<span style="color:gray;">a</span>)&nbsp;{
		<span style="color:blue;">if</span>&nbsp;(<span style="color:gray;">a</span>.second.is&lt;<span style="color:blue;">char</span>&gt;())
			actions.push_back(<span style="color:gray;">a</span>.second.as&lt;<span style="color:blue;">char</span>&gt;());
	};
	<span style="color:green;">//I&nbsp;am&nbsp;lazy</span>
	<span style="color:blue;">auto</span>&nbsp;t&nbsp;=&nbsp;<span style="color:gray;">o</span>.second.as&lt;sol::<span style="color:#2b91af;">table</span>&gt;();
	<span style="color:blue;">if</span>&nbsp;(t<span style="color:teal;">[</span>6<span style="color:teal;">]</span>.get_type()&nbsp;==&nbsp;sol::<span style="color:#2b91af;">type</span>::<span style="color:darkslategray;">table</span>)
		t<span style="color:teal;">[</span>6<span style="color:teal;">]</span>.get&lt;sol::<span style="color:#2b91af;">table</span>&gt;().for_each(getActions);
	scene_temp-&gt;addObject(t<span style="color:teal;">[</span>1<span style="color:teal;">]</span>.get_or&lt;std::<span style="color:#2b91af;">string</span>&gt;(<span style="color:#a31515;">&quot;r0,0&quot;</span>),&nbsp;sf::<span style="color:#2b91af;">Vector3i</span>(t<span style="color:teal;">[</span>2<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>3<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>4<span style="color:teal;">]</span>.get_or(0)),&nbsp;t<span style="color:teal;">[</span>5<span style="color:teal;">]</span>.get_or&lt;std::<span style="color:#2b91af;">string</span>&gt;(<span style="color:#a31515;">&quot;&quot;</span>),&nbsp;actions);
}
 
<span style="color:blue;">void</span>&nbsp;<span style="color:#2b91af;">Lua</span>::readWalkbox(std::<span style="color:#2b91af;">pair</span>&lt;sol::<span style="color:#2b91af;">object</span>,&nbsp;sol::<span style="color:#2b91af;">object</span>&gt;&nbsp;<span style="color:gray;">o</span>)&nbsp;{
	<span style="color:blue;">if</span>&nbsp;(!<span style="color:gray;">o</span>.second.is&lt;sol::<span style="color:#2b91af;">table</span>&gt;())
		<span style="color:blue;">throw</span>&nbsp;<span style="color:#2b91af;">Exception</span>::<span style="color:darkslategray;">noObjectTable</span>;
 
	<span style="color:blue;">auto</span>&nbsp;t&nbsp;=&nbsp;<span style="color:gray;">o</span>.second.as&lt;sol::<span style="color:#2b91af;">table</span>&gt;();
	scene_temp-&gt;addWalkbox(sf::<span style="color:#2b91af;">IntRect</span>(t<span style="color:teal;">[</span>1<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>2<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>3<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>4<span style="color:teal;">]</span>.get_or(0)),&nbsp;t<span style="color:teal;">[</span>5<span style="color:teal;">]</span>.get_or(<span style="color:blue;">true</span>));
}
 
<span style="color:blue;">void</span>&nbsp;<span style="color:#2b91af;">Lua</span>::readZoomline(std::<span style="color:#2b91af;">pair</span>&lt;sol::<span style="color:#2b91af;">object</span>,&nbsp;sol::<span style="color:#2b91af;">object</span>&gt;&nbsp;<span style="color:gray;">o</span>)&nbsp;{
	<span style="color:blue;">if</span>&nbsp;(!<span style="color:gray;">o</span>.second.is&lt;sol::<span style="color:#2b91af;">table</span>&gt;())
		<span style="color:blue;">throw</span>&nbsp;<span style="color:#2b91af;">Exception</span>::<span style="color:darkslategray;">noObjectTable</span>;
 
	<span style="color:blue;">auto</span>&nbsp;t&nbsp;=&nbsp;<span style="color:gray;">o</span>.second.as&lt;sol::<span style="color:#2b91af;">table</span>&gt;();
	scene_temp-&gt;addZoomline(sf::<span style="color:#2b91af;">IntRect</span>(t<span style="color:teal;">[</span>1<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>2<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>3<span style="color:teal;">]</span>.get_or(0),&nbsp;t<span style="color:teal;">[</span>4<span style="color:teal;">]</span>.get_or(0)),&nbsp;t<span style="color:teal;">[</span>5<span style="color:teal;">]</span>.get_or&lt;<span style="color:blue;">float</span>&gt;(0),&nbsp;t<span style="color:teal;">[</span>6<span style="color:teal;">]</span>.get_or(<span style="color:blue;">true</span>));
}</pre>

As you see this almost feels like Lua access to a variable. All we must do is to call a specific method of the scene. Now is the time to add the objects to the rendering list and voila: Done. 
Special case: Object without texture
As you saw earlier we had one line which was different: 

<pre style="font-family:Consolas;font-size:13;color:black;background:white;">{<span style="color:#a31515;">&quot;r800,600&quot;</span>,&nbsp;200,&nbsp;100,&nbsp;1,&nbsp;<span style="color:#a31515;">&quot;hello&quot;</span>,&nbsp;{<span style="color:#a31515;">&#39;u&#39;</span>,&nbsp;<span style="color:#a31515;">&#39;l&#39;</span>}}
</pre>

We don’t add a texture here, as we just want to have a callback. I could have used a transparent texture, but then, we would add an unseen texture to the rendering, which would kill our performance, when used heavily. I chose to use our String to define, that this is just a rectangle we can click and get our callback. The first character is a `r`: It tells the engine: Just a callback rectangle without a texture is wanted. The following, by a comma separated values are defining width and height. This rectangle is added as object but won’t get added to the draw list. The benefit is, we can send the callback without iterating twice and we save rendering time. 

# How it works – The “Editor Mode” #
For getting the in-engine editor nearer, I created a so called “editor mode”. You can switch it via pressing `F3`. In this mode we see all the objects (red), walkboxes (green) and zoomlines (blue). To get this displayed I iterate through the scene and save colored rectangles to a list. This list is then added to the GUI draw list, which is like the draw list, but reserved for the GUI. It simply gets drawn after the normal draw list, so our rectangles are always in the foreground. Since it’s pointless to always close and reload the engine when changing the scenes, we can press `Shift+F3` to reload the “Engine.lua” and so the scene. As a bonus, if you failed writing proper Lua, the engine just won’t display anything instead of crashing, unlike in the startup.
![The editor mode]({{"/assets/2-editor-mode.png" | absolute_url}})

# What comes next? #
The next step is allowing multiple scenes being loaded, so we can switch them easily. With that I take the first scene to the “Engine.lua” so we can start off capsuling Game and Engine completely. Afterwards I will add some functions to control the engine from Lua like a scene changer, displaying a subtitle or playing a sound. Once that is finished I start making some editor tools to get closer to an in-engine editor. The next blog post will be Saturday or Sunday, but eventually about the future of the blog, because next week starts school again and I won’t have the time I have right now.

# End Quote #
>The best refactoring after a long period is perhaps a restart.
>
> --HaSa (Hopefully the last one from me)
