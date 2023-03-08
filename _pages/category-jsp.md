---
title: 'JSP'
layout: category
permalink: /categories/jsp
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.jsp %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}