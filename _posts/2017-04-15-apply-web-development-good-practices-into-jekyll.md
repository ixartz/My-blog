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
  - Fingerprints
  - Continuous integration
  - Continuous deployment
  - Content delivery network
  - Https
  - Good practice
---
Lately, I was working to enable an HTTPS-encrypted connection to my Jekyll blog. For your information, it is currently hosted on GitHub Pages. But, I end up by applying a lot of web development good practice into a static blog:

* Manage a list of dependencies with a Gemfile
* Asset fingerprints
* Implement a Ruby task runner, Rake
* Minify CSS/JS
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

    $ gemrat jekyll jekyll-minibundle rake html-proofer

In the next section, we will cover each dependency and you will have a quick understanding when we start using them.

Unfortunately, the above command is to enough to install the dependencies. Indeed, we have just indicated that this project needs them. In order to run the installation, you will need to run:

    $ bundle install

After that, all dependencies we need for this tutorial will be available in our project.

## Custom plugins on GitHub Pages

We have installed a plugin named *jekyll-minibundle*. For security reasons, GitHub have disabled custom plugins. It means we cannot use *jekyll-minibundle* if we deploy directly to GitHub Pages. The only way is to generated the output static files and push them to your repository.

The reason we use use *jekyll-minibundle* is it provides an asset fingerprint. An asset fingerprint is to control caching of static resources. We will go deeper about this practice in the next section. *jekyll-minibundle* also implements an asset bundle but in this article, we will not use it because we will depend on Sass. Indeed, we can indicate to Sass compiler to pack all your files into one output CSS file and compress it.

## Asset fingerprint

After installing dependencies, we need to configure our Jekyll in order to load *jekyll-minibundle*. Open *_config.yml* and add this following code at the end:

{% highlight yml %}
gems: [jekyll/minibundle]
{% endhighlight %}

Then, find all the location in your project to add the fingerprint:

## Use Rake as task runner

For those who do not know what is a task runner, it is a tool performing a repetitive task. Here, we will implement tasks into the Rakefile:

    * Build your scss files
    * Build your Jekyll site/blog
    * Test if your generated files are valid
