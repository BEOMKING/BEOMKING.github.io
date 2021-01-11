---
title: "그리디 알고리즘"
layout: archive
permalink: categories/ml
author_profile: true
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories.['a b c'] 이런식으로! -->

***

{% assign posts = site.categories.['Machine Learning'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}

[comment]: <> (카테고리 클릭시 해당 카테고리로 이동 후 커스터마이징된 화면 출력)