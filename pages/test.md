---
layout: page
title: Test
description: 人越学越觉得自己无知
keywords: 测试页, Test
comments: false
menu: 测试
permalink: /test/
---

> 记多少命令和快捷键会让脑袋爆炸呢？

<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
<li class="listing-item"><a href="{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
</ul>



