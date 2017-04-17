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

* Manage a list of dependencies with a Gemfile
* Implement a Ruby task runner, Rake
* Minify CSS/JS
* Asset fingerprinting
* Continuous integration
* Continuous deployment
* Content delivery network
* An HTTP secure communication

In this article, I will explain how to set these elements in your favorite static generator, Jekyll.

## Install dependencies

*You should have already installed Jekyll in your local environment. If it is not the case, you can <a href="https://jekyllrb.com/docs/installation/" target="_blank">follow this tutorial</a>.*

The first step requires you to install Bundler and Gamrat with <a href="https://rubygems.org" target="_blank">RubyGems</a>:

    $ gem install bundler gemrat
