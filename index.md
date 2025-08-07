---
layout: default
title: agl-alexglopez
---

## Open Source Learning

This blog is a documentation of valuable lessons I have learned and new ideas I am exploring related to programming, algorithms, data structure design, operating systems, compilers, and many more topics to come. The site and its posts aim to be minimal in design, free from distractions, and full of useful information. 

Please check out any posts below that interest you.

## Corrections

I aim to present correct information in my posts. If you notice any errors, or are otherwise compelled to improve a post, please feel free to submit an issue at the [repository for this site](https://github.com/agl-alexglopez/agl-alexglopez.github.io).  

## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small>— {{ post.date | date: "%b %-d, %Y" }}</small>
    </li>
  {% endfor %}
</ul>
