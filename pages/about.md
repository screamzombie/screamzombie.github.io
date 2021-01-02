---
layout: page
title: 关于
menu: 关于
permalink: /about/
---

## 关于我
姓名:
  林登万

英文名:
  Leader One

常用昵称:
  Rainbow,ScreamZombie

性别:
  男

生日:
  2001.9.23

职业:
  coder

QQ:
  1525025692

电子邮箱:
  sudorainbowcat@gmail.com
  1525025692@qq.com

坐标:
  Heifei, Anhui

爱好：

写代码,看番,吹逼,搞事情,演讲

联系
{% for website in site.data.social %}

{{ website.sitename }}：[@{{ website.name }}]({{ website.url }}) {% endfor %}
Skill Keywords
{% for category in site.data.skills %}

{{ category.name }}
{% for keyword in category.keywords %} {{ keyword }} {% endfor %}
{% endfor %}
