---
title: "플레이리스트"
layout: archive
permalink: categories/personal/playlist
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Playlist %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}