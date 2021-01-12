---
title: "일상"
layout: archive
permalink: categories/daily
author_profile: true
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories.['a b c'] 이런식으로! -->

***

{% assign posts = site.categories.daily %} <!-- nav_list_main에서 적은 카테고리명과 같아야함 -->
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}