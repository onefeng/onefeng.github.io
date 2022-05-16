---
layout: page
title: 关于
description: 代码江湖
keywords: onefeng, 晚风
comments: false
menu: 关于
permalink: /about/
---

我是晚风/onefeng

以码会友，崇尚码德、工程师精神，有一定的代码洁癖。

## 简介

希望能与各位交流学习，平时写的博客主要是为了记录一些重要的内容，和解决问题的记录。
不能保证所有人都能看得懂~~\
其他就没有什么了。\Talk is cheap,Show me the code.

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a target="_blank">{{ website.name }}</a></li>
{% endfor %}
</ul>
