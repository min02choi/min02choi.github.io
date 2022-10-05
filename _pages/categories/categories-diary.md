---
title: "나의 일기장"
layout: archive
permalink: categories/etc
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Diary %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}