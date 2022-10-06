---
title: "일기장"
layout: archive
permalink: categories/personal
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Daily %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}