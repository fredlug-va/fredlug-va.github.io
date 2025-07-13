---
layout: default
title: Home
---
<div class="home">

  <h1 class="page-heading">Meetings</h1>

  {%- assign allPosts = site.posts -%}

  <p>There are {{ allPosts.size }} posts.</p>

  <ul class="post-list">
    {% for post in allPosts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
        
      </li>
    {% endfor %}
  </ul>

  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

</div>