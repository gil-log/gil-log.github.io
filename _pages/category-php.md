---
title: 'PHP'
layout: category
permalink: /categories/php
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.php %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}