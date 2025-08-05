---
layout: default
title: agl-alexglopez
---

## Open Source Learnings

This blog is a documentation of valuable lessons I have learned and new ideas I am exploring related to programming, algorithms, data structure design, operating systems, compilers, and many more topics as they interest me. The site and its posts aim to be minimal in design, free from distractions, and full of useful information. 

Please check out any posts below that interest you.


## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small>— {{ post.date | date: "%b %-d, %Y" }}</small>
    </li>
  {% endfor %}
</ul>
