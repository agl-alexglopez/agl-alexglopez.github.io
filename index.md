---
layout: default
title: Home
---

# My Open Source Learnings

This blog is a documentation of valuable lessons I have learned and new ideas I am exploring. The site and its posts aim to be minimal in design, free from distractions, and full of information. Please check out any posts below that interest you.

## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small>— {{ post.date | date: "%b %-d, %Y" }}</small>
    </li>
  {% endfor %}
</ul>
