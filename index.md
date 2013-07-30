---
layout: page
title: 明无梦的博客
sitemap:
  priority: 1.0
---

## 最近的10篇文章

<ul class="post">
  {% for post in site.posts limit:10 %}
  <li><span>{{ post.date | date: "%Y-%m-%d" }}</span> &raquo;
  <span class="post-tag">
    <a href="{{ site.categories_path }}#{{ post.category }}">{{ post.category }}</a>
  </span>
  <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## 关于明无梦

未来，我做了一个名为“无”的梦。
过去，也曾做过。
我不知现在是否在做梦。
可能一切本身就是无吧。
世界本是无，是我们把无当成了有，其实不都一样吗？  
——写于2011年4月30日19:21:32

## 订阅我

[RSS](http://www.dreamxu.com/rss.xml)  
<a target="_blank" href="http://list.qq.com/cgi-bin/qf_invite?id=fd21b3769bda4d0cd91656847d2e2cd3dd7a6e5effdedec9">EMAIL</a>

## 联系我

邮箱 : <mwum@dreamxu.com>
