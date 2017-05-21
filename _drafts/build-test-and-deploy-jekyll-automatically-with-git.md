---
title: 'Build, test and deploy Jekyll automatically with Git'
description: Jekyll continuous integration and deployment
categories:
  - Jekyll
tags:
  - GitHub
  - Travis CI
  - Continuous integration
  - Continuous deployment
---
In my previous article <a href="{{ site.baseurl }}how-to-install-a-custom-plugin-in-Jekyll/" target="_blank">on how to install a custom plugin in Jekyll</a>, we have successfully added an asset fingerprints plugin. Unfortunately, we need to run manually some commands to build, test and deploy Jekyll to GitHub Pages.

We can avoid this pain by running all the steps when we make a Git commit and push. In this tutorial, we set up a continuous integration and deployment for Jekyll with <a href="https://travis-ci.org/" target="_blank">Travis CI</a>.

## Basic configuration

Let's make a *Hello world* on Ruby with Travis CI. Firstly, we need to create a *.travis.yml* file and add the following code into it:

{% highlight yml %}
language: ruby

rvm:
  - 2.3.3

script: ruby -e "puts 'Hello world'"

sudo: false
{% endhighlight %}

In *.travis.yml*, we can choose the language and the version we want to use. Here, we will use Ruby *2.3.3*. Mostly because Jekyll is written in Ruby. And, we use *2.3.3* because this version is already available in the build environment.

## Continuous integration

The first step of the script is to build our Jekyll site automatically after pushing a commit.

After successfully build our Jekyll, we need to test if the files are correctly generated.

## Continuous deployment

Good news! Travis CI have already provided a method to deploy easily on GitHub Pages.
