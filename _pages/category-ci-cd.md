---
title: 'CI/CD'
layout: category
permalink: /categories/ci-cd
author_profile: true
sidebar_main: true
---
{% assign posts = site.categories.ci-cd %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
