---
title: HaSa's Blog
layout: default
comments: false
---

<p>Welcome to my blog! I am posting here my progress on my Point and Click Adventure Engine (ITLengine) and other stuff I come across. Feel free to leave a comment or post an issue in the Github Repo. Have Fun Reading!</p>
  
  <h1>Blog Posts</h1>
  {% for post in site.posts %}
  <article>
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    <small>
    Tags:{% capture tags %} {% for tag in post.tags %}, {{tag}}{% endfor %}{% endcapture %} {{tags | remove_first: ", "}}<br>
    <a class="disqus-comment-count" href={{post.url | append: "#disqus_thread"}} data-disqus-url="{{page.url}}">0 Comments</a></small>

  * * *

  </article>
  {% endfor %}
