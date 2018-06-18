---
title: "#4 Progress in the Engine"
date: 2018-06-18 22:00:00
tags: Lua C++ ITLengine worklog personal blog
layout: default
comments: true
---

# Content #
1. General Changes
2. The Changes in the Engine
  1. Animations
  2. Movable Objects
  3. Separating Lua from the Engine
  4. Using function binding instead of table4.checking
3. What's next?
  1. Engine
  2. Game

### Images missing. Sorry :( ###
# General Changes #
Dear readers,
as you may noticed the last post was a few more weeks ago. That's because I am packed with projects and I had just a little progress on the engine, since I also worked at other projects. All that leads me to my next point, which is obviously the blog. Today I officially announce that I am not going to even try to belong to my pattern. I may create a rss feed so that you can subscribe to one and see if a new post came.

# The Changes in the Engine #
The engine got more object-oriented. So I created several classes:
## Animations ##
Animations are just a holder for states and the current state. That is because the engine needs the "sprite-sheet" and then it saves where is what state, so you can simply change it.
## Movable Objects #
The movable object is a object which is used to display movements. What a silly sentence. However, you define points, where you want to move and areas to define which state to show. Here will be an example:

## Separating Lua from the Engine ##
... is the latest and possibly biggest milestone I achieved. I wrote an abstract class to define all the functions you need to have in your language implementation. In the engine header you only have to change the following. You don't need to change anything in the engine.cpp as long as you bind the C++ functions directly to your language.

<pre class="vs-code"><span class="cppType">Lua</span>              <span class="cppMemberField">script</span><span class="operator">;</span>      <span class="comment">///&lt;&nbsp;The&nbsp;ScriptEngine</span>
<span class="cppType">ChangeThisToAnythingNeeded</span>  <span class="cppMemberField">script</span><span class="operator">;</span>      <span class="comment">///&lt;&nbsp;The&nbsp;ScriptEngine</span></pre>

## Using function binding instead of table-checking ##
With the mentioned change I changed the interaction between Lua (in my case) and the engine to use function binding. That is almost needed to kick out Lua of the engine control. The side-effect is that our code got way cleaner and easier to read. That's nice.

# What's next? #
## Engine ##
1. The Engine is getting support for static animations. This might be fast done, since we should have already everything.
2. Over2.sized scenes is one the top of the toDo2.list, since we will soon need them. This could be a bit complicated, since I first have to implement characters, and then camera and character control.
3. Character support, as mentioned above.
4. A better debug mode, with more features, like directly editing areas and points for animations, directly setting the states in the engine with rectangles, editing of walkboxes and zoomlines
5. A completely customizable GUI, via Lua or XMl or both or...
6. Items, you need to collect stuff in an point and click adventure
7. Cutscenes
8. finally I will add savegame and inventory, since I don't want to change it every second hour.
9. Well we might need to take a look at the performance, too

## Game ##
Currently I am writing the story of the game and delegating work to get soon a prototype. Once we get something showable I will write another post.

## Next Post(s) ##
I am planning a post about "Detroit: Become Human" and my view on there effort for point and click adventures.


Thanks for Reading.
