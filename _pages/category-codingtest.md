---
title: 'CodingTest'
layout: category
permalink: /categories/codingtest
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.codingtest %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}