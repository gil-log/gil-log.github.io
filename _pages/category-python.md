---
title: 'Python'
layout: category
permalink: /categories/python
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.python %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}