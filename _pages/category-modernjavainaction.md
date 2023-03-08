---
title: 'ModernJavaInAction'
layout: category
permalink: /categories/modernjavainaction
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.modernjavainaction %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}