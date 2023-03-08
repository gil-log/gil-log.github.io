---
title: 'SideProject'
layout: category
permalink: /categories/sideproject
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.sideproject %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}