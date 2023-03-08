---
title: 'Text Mining'
layout: category
permalink: /categories/textmining
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.textmining %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}