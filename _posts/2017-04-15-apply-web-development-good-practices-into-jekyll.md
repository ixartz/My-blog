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
* Use a static code analyzer, Rubocop
* Minify CSS/JS
* Asset fingerprinting
* Continuous integration
* Continuous deployment
* Content delivery network
* An HTTP secure communication

In this article, I will explain how to set these elements in your favorite static generator, Jekyll.

## Install dependencies

*You should have already installed Jekyll in your local environment. If it is not the case, you can <a href="https://jekyllrb.com/docs/installation/" target="_blank">follow this tutorial</a>.*

The first step requires you to install Bundler and Gemrat with <a href="https://rubygems.org" target="_blank">RubyGems</a>:

    $ gem install bundler gemrat

In this project, we need to use several dependencies. These dependencies are stored into a Gemfile. We will fill this file using Gemrat that we have previously install:

    $ gemrat jekyll jekyll-minibundle rake rubocop html-proofer

In the next section, we will cover each dependency and you will have a quick understanding when we start using them.

Unfortunately, the above command is to enough to install the dependencies. Indeed, we have just indicated that this project needs them. In order to run the installation, you will need to run:

    $ bundle install

After that, all dependencies we need for this tutorial will be available in our project.