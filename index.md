---
title: 联机托管说明
permalink: index.html
layout: home
---

## 内容目录

下面列出了每个实验室的超链接。

## 实验

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| 模块 | 实验室 |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}

