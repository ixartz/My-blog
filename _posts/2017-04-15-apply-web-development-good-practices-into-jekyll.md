---
title: 'Apply web development good practices into Jekyll'
date: 2017-04-15 23:18:24
description: Your Jekyll websites also deserves your attention
categories:
  - Jekyll
tags:
  - GitHub
  - Rake
  - Gemfile
  - Minify
  - fingerprinting
  - Continuous integration
  - Continuous deployment
  - Content delivery network
  - Https
  - Good practice
---
Lately, I was working to enable an HTTPS-encrypted connection to my Jekyll blog. For your information, it is currently hosted on GitHub Pages. But, I end up by applying a lot of web development good practice into a static blog:

* Implement a Ruby task runner, Rake
* Manage a list of dependencies with a Gemfile
* Minify CSS/JS
* Asset fingerprinting
* Continuous integration
* Continuous deployment
* Content delivery network
* An HTTP secure communication
