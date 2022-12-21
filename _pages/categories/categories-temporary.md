---
title: "Temporary"
layout: archive
permalink: categories/etc/temporary
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Temporary %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}