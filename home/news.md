---
layout: default
title: News
permalink: /news/index.html
---




<div class="newsentries">

  {% for post in site.posts %}
  <a href="{{ post.url }}">
    <br/>
    <br/>
    <h1 class="newstitle">
      {{ post.title }}
    </h1>
  </a>
  <div class="newsinfo">
    <img width="15" height="15" src="/resources/images/calendar.png"/>&nbsp; {{ post.date | date: "%d.%m.%Y." }}, poster: {{ post.poster }}
  </div>
  <br/>
  {{ post.content }}
  {% endfor %}
  
</div>






