---
title:  "#2 The Basic Engine"
date:   2018-04-04 18:30:00 +0200
tags: ITLengine worklog lua c++
layout: default
comments: true
---

As I said yesterday, today is the moment, I show a bit of the engine of today’s build. It will get technical, but if it gets un-understandable tell me.

# How it works – The Lua Part #
We have a little Lua to initialize the engine with all the needed tables. If they aren’t existing, the engine is hopelessly lost.

<pre class="vs-code">scenes&nbsp;<span class="operator">=</span>&nbsp;{}
localisations&nbsp;<span class="operator">=</span>&nbsp;{}
game&nbsp;<span class="operator">=</span>&nbsp;{}</pre>

As you see we have a scenes table, which obviously holds all the scenes, a localizations table which isn’t needed really in the engine now and finally a game table.
I use the game table for saving configurations of the game, like the language or other game related settings.
Once we initialized the tables we fill them.

<pre class="vs-code"><span class="keyword">dofile</span>&nbsp;<span class="string">&quot;locals/en.lang&quot;</span>
<span class="keyword">dofile</span>&nbsp;<span class="string">&quot;locals/de.lang&quot;</span>
 
<span class="keyword">dofile</span>&nbsp;<span class="string">&quot;scenes/lua_testscene.pcscene&quot;</span>
 
<span class="comment">--&nbsp;Load&nbsp;the&nbsp;configs
</span>game.<span class="identifier">stdlang</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="string">&quot;en&quot;</span>
game.<span class="identifier">lang</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="string">&quot;en&quot;</span>
 
<span class="keyword">dofile</span>&nbsp;<span class="string">&quot;Game.conf&quot;</span></pre>

That’s the moment it gets annoying: We must load every single file, and as I am to lazy to write a function, which iterates through a folder, we must add them manually to the file or write them as you wish. All the engine internally does is to execute this single “Engine.lua”. Everything else is in your hand! Let’s see how a scene file looks like:

# How it works – The Scene File #

<pre class="vs-code">scenes[<span class="string">&quot;lua_testscene&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{}
<span class="comment">--&nbsp;We&nbsp;are&nbsp;lazy
</span><span class="keyword">local</span>&nbsp;scene&nbsp;<span class="operator">=</span>&nbsp;scenes[<span class="string">&quot;lua_testscene&quot;</span>]
 
<span class="comment">--&nbsp;The&nbsp;Objects&nbsp;now:&nbsp;Format&nbsp;{texture&nbsp;path,&nbsp;position:&nbsp;x,y,z,&nbsp;callback&nbsp;(or&nbsp;nil),&nbsp;{actions&nbsp;(ether&nbsp;u&nbsp;or&nbsp;c)&nbsp;and/or&nbsp;l}&nbsp;(or&nbsp;nil)}
</span>scene[<span class="string">&quot;objects&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
	{<span class="string">&quot;.\\textures\\test.png&quot;</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="string">&quot;bin&quot;</span>,&nbsp;{<span class="string">&#39;u&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}},
	<span class="comment">--&nbsp;This&nbsp;one&nbsp;is&nbsp;special:&nbsp;It&nbsp;gets&nbsp;explained&nbsp;later
</span>	{<span class="string">&quot;r800,600&quot;</span>,&nbsp;<span class="number">200</span>,&nbsp;<span class="number">100</span>,&nbsp;<span class="number">1</span>,&nbsp;<span class="string">&quot;hello&quot;</span>,&nbsp;{<span class="string">&#39;u&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}}
}
 
<span class="comment">--&nbsp;The&nbsp;Walkboxes:&nbsp;Format:&nbsp;{position:&nbsp;x,y,&nbsp;width,&nbsp;height,&nbsp;isActive&nbsp;(bool)}
</span>scene[<span class="string">&quot;walkboxes&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
	{<span class="number">0</span>,&nbsp;<span class="number">10</span>,&nbsp;<span class="number">1000</span>,&nbsp;<span class="number">400</span>,&nbsp;<span class="keyword">true</span>},
	{<span class="number">200</span>,&nbsp;<span class="number">10</span>,&nbsp;<span class="number">1000</span>,&nbsp;<span class="number">400</span>,&nbsp;<span class="keyword">true</span>},
	{<span class="number">150</span>,&nbsp;<span class="number">300</span>,&nbsp;<span class="number">60</span>,&nbsp;<span class="number">200</span>,&nbsp;<span class="keyword">true</span>}
}
 
<span class="comment">--&nbsp;The&nbsp;Zoomlines:&nbsp;Format&nbsp;{point1:&nbsp;x,y,&nbsp;point2:&nbsp;x,y,&nbsp;zoomfactor,&nbsp;isActive}
</span>scene[<span class="string">&quot;zoomlines&quot;</span>]&nbsp;<span class="operator">=</span>&nbsp;{
	{<span class="number">0</span>,&nbsp;<span class="number">0</span>,&nbsp;<span class="number">1000</span>,&nbsp;<span class="number">1000</span>,&nbsp;<span class="number">0.9</span>,&nbsp;<span class="keyword">true</span>}
}
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.hello</span><span class="operator">(</span><span class="identifier">&nbsp;action&nbsp;</span><span class="operator">)</span>
	<span class="comment">--&nbsp;display&nbsp;hello&nbsp;testy&nbsp;World&nbsp;
</span>	<span class="keyword">print</span>(<span class="keyword">display</span>(<span class="string">&quot;test&quot;</span>));&nbsp;<span class="comment">--&nbsp;test
</span><span class="keyword">end</span>
 
<span class="keyword">function</span>&nbsp;<span class="type">scene.bin</span><span class="operator">(</span><span class="identifier">&nbsp;action&nbsp;</span><span class="operator">)</span>
	<span class="comment">--&nbsp;display&nbsp;depending&nbsp;on&nbsp;action&nbsp;runs&nbsp;or&nbsp;it&#39;s&nbsp;a&nbsp;tablet
</span>	<span class="keyword">if</span>&nbsp;action&nbsp;<span class="operator">==</span>&nbsp;<span class="string">&#39;u&#39;</span>&nbsp;<span class="keyword">then</span>&nbsp;<span class="keyword">print</span>(<span class="keyword">display</span>(<span class="string">&quot;tabletU&quot;</span>))
	<span class="keyword">elseif</span>&nbsp;action&nbsp;<span class="operator">==</span>&nbsp;<span class="string">&#39;l&#39;</span>&nbsp;<span class="keyword">then</span>&nbsp;<span class="keyword">print</span>(<span class="keyword">display</span>(<span class="string">&quot;tabletL&quot;</span>))
	<span class="keyword">end</span>
<span class="keyword">end</span>
 
<span class="keyword">return</span>&nbsp;scene</pre>

This simple file results in this simple scene:
![Result]({{"/assets/2-result-scene.png" | absolute_url}})
Even the callbacks are working:

![Callbacks]({{"/assets/2-output.png" | absolute_url}})

Once we finished the loading of the file we are loading this specific scene - this get changed, don’t worry! Now we get to the interesting stuff in the engine:

# How it works – The Static Part #
Once Lua is done my engine gratefully says thanks and adds the objects, walk boxes, and zoom lines. To a scene which it got from the engine manager (better known as Engine).

<pre class="vs-code"><span class="comment">//first&nbsp;is&nbsp;the&nbsp;key,&nbsp;second&nbsp;the&nbsp;value</span>
<span class="keyword">void</span>&nbsp;<span class="cppType">Lua</span><span class="operator">::</span><span class="cppMemberFunction">readObject</span><span class="operator">(</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">pair</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">,</span>&nbsp;<span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">&gt;</span>&nbsp;<span class="cppParameter">o</span><span class="operator">)</span>&nbsp;<span class="operator">{</span>
	<span class="keyword">if</span>&nbsp;<span class="operator">(!</span><span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">is</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;())</span>
		<span class="keyword">throw</span>&nbsp;<span class="cppType">Exception</span><span class="operator">::</span><span class="cppEnumerator">noObjectTable</span><span class="operator">;</span>&nbsp;<span class="comment">//OOops&nbsp;the&nbsp;Lua&nbsp;was&nbsp;wrong</span>
 
	<span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">list</span><span class="operator">&lt;</span><span class="keyword">char</span><span class="operator">&gt;</span>&nbsp;<span class="cppLocalVariable">actions</span><span class="operator">;</span>
	<span class="comment">//We&nbsp;need&nbsp;to&nbsp;read&nbsp;the&nbsp;actions,&nbsp;because&nbsp;it&#39;s&nbsp;a&nbsp;table</span>
	<span class="keyword">auto</span>&nbsp;<span class="cppLocalVariable">getActions</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="operator">[&amp;</span><span class="cppLocalVariable">actions</span><span class="operator">](</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">pair</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">,</span>&nbsp;<span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">&gt;</span>&nbsp;<span class="cppParameter">a</span><span class="operator">)</span>&nbsp;<span class="operator">{</span>
		<span class="keyword">if</span>&nbsp;<span class="operator">(</span><span class="cppParameter">a</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">is</span><span class="operator">&lt;</span><span class="keyword">char</span><span class="operator">&gt;())</span>
			<span class="cppLocalVariable">actions</span><span class="operator">.</span><span class="cppMemberFunction">push_back</span><span class="operator">(</span><span class="cppParameter">a</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">as</span><span class="operator">&lt;</span><span class="keyword">char</span><span class="operator">&gt;());</span>
	<span class="operator">};</span>
	<span class="comment">//I&nbsp;am&nbsp;lazy</span>
	<span class="keyword">auto</span>&nbsp;<span class="cppLocalVariable">t</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">as</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;();</span>
	<span class="keyword">if</span>&nbsp;<span class="operator">(</span><span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">6</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_type</span><span class="operator">()</span>&nbsp;<span class="operator">==</span>&nbsp;<span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">type</span><span class="operator">::</span><span class="cppEnumerator">table</span><span class="operator">)</span>
		<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">6</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;().</span><span class="cppMemberFunction">for_each</span><span class="operator">(</span><span class="cppLocalVariable">getActions</span><span class="operator">);</span>
	<span class="cppMemberField">scene_temp</span><span class="operator">-&gt;</span><span class="cppMemberFunction">addObject</span><span class="operator">(</span><span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">1</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">&lt;</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">string</span><span class="operator">&gt;(</span><span class="string">&quot;r0,0&quot;</span><span class="operator">),</span>&nbsp;<span class="cppNamespace">sf</span><span class="operator">::</span><span class="cppType">Vector3i</span><span class="operator">(</span><span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">2</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">3</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">4</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">)),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">5</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">&lt;</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">string</span><span class="operator">&gt;(</span><span class="string">&quot;&quot;</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">actions</span><span class="operator">);</span>
<span class="operator">}</span>
 
<span class="keyword">void</span>&nbsp;<span class="cppType">Lua</span><span class="operator">::</span><span class="cppMemberFunction">readWalkbox</span><span class="operator">(</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">pair</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">,</span>&nbsp;<span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">&gt;</span>&nbsp;<span class="cppParameter">o</span><span class="operator">)</span>&nbsp;<span class="operator">{</span>
	<span class="keyword">if</span>&nbsp;<span class="operator">(!</span><span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">is</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;())</span>
		<span class="keyword">throw</span>&nbsp;<span class="cppType">Exception</span><span class="operator">::</span><span class="cppEnumerator">noObjectTable</span><span class="operator">;</span>
 
	<span class="keyword">auto</span>&nbsp;<span class="cppLocalVariable">t</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">as</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;();</span>
	<span class="cppMemberField">scene_temp</span><span class="operator">-&gt;</span><span class="cppMemberFunction">addWalkbox</span><span class="operator">(</span><span class="cppNamespace">sf</span><span class="operator">::</span><span class="cppType">IntRect</span><span class="operator">(</span><span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">1</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">2</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">3</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">4</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">)),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">5</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="keyword">true</span><span class="operator">));</span>
<span class="operator">}</span>
 
<span class="keyword">void</span>&nbsp;<span class="cppType">Lua</span><span class="operator">::</span><span class="cppMemberFunction">readZoomline</span><span class="operator">(</span><span class="cppNamespace">std</span><span class="operator">::</span><span class="cppType">pair</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">,</span>&nbsp;<span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">object</span><span class="operator">&gt;</span>&nbsp;<span class="cppParameter">o</span><span class="operator">)</span>&nbsp;<span class="operator">{</span>
	<span class="keyword">if</span>&nbsp;<span class="operator">(!</span><span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">is</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;())</span>
		<span class="keyword">throw</span>&nbsp;<span class="cppType">Exception</span><span class="operator">::</span><span class="cppEnumerator">noObjectTable</span><span class="operator">;</span>
 
	<span class="keyword">auto</span>&nbsp;<span class="cppLocalVariable">t</span>&nbsp;<span class="operator">=</span>&nbsp;<span class="cppParameter">o</span><span class="operator">.</span><span class="cppMemberField">second</span><span class="operator">.</span><span class="cppMemberFunction">as</span><span class="operator">&lt;</span><span class="cppNamespace">sol</span><span class="operator">::</span><span class="cppType">table</span><span class="operator">&gt;();</span>
	<span class="cppMemberField">scene_temp</span><span class="operator">-&gt;</span><span class="cppMemberFunction">addZoomline</span><span class="operator">(</span><span class="cppNamespace">sf</span><span class="operator">::</span><span class="cppType">IntRect</span><span class="operator">(</span><span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">1</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">2</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">3</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">4</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="number">0</span><span class="operator">)),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">5</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">&lt;</span><span class="keyword">float</span><span class="operator">&gt;(</span><span class="number">0</span><span class="operator">),</span>&nbsp;<span class="cppLocalVariable">t</span><span class="cppMemberOperator">[</span><span class="number">6</span><span class="cppMemberOperator">]</span><span class="operator">.</span><span class="cppMemberFunction">get_or</span><span class="operator">(</span><span class="keyword">true</span><span class="operator">));</span>
<span class="operator">}</span></pre>

As you see this almost feels like Lua access to a variable. All we must do is to call a specific method of the scene. Now is the time to add the objects to the rendering list and voila: Done. 
# Special case: Object without texture #
As you saw earlier we had one line which was different: 

<pre class="vs-code">{<span class="string">&quot;r800,600&quot;</span>,&nbsp;<span class="number">200</span>,&nbsp;<span class="number">100</span>,&nbsp;<span class="number">1</span>,&nbsp;<span class="string">&quot;hello&quot;</span>,&nbsp;{<span class="string">&#39;u&#39;</span>,&nbsp;<span class="string">&#39;l&#39;</span>}}
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
