---
title: 'How to install a custom plugin in Jekyll'
date: 2017-04-15 23:18:24
description: Asset fingerprint for Jekyll
categories:
  - Jekyll
---
In this article, I will explain how to install a custom plugin in your favorite static generator, Jekyll. At least, it is my favorite one. We will also use this custom plugin on GitHub Pages.

This tutorial will be based on a real case. Indeed, the goal is to have an asset fingerprint for Jekyll. Do not worry if you do not know what is the asset fingerprint. It is a way to control browser cache and we will go deeper about this technique in the following section.

## Custom plugins on GitHub Pages

We will install a plugin named *jekyll minibundle*. For security reasons, GitHub have disabled custom plugins. It means we cannot use it directly if we deploy to GitHub Pages.

The only way is to generate the static files in your local machine and push them to your GitHub repository.

### Install dependencies

*You should have already installed Jekyll in your local environment. If it is not the case, you can <a href="https://jekyllrb.com/docs/installation/" target="_blank">follow this tutorial</a>.*

The first step requires you to install Bundler and Gemrat with <a href="https://rubygems.org" target="_blank">RubyGems</a>:

    $ gem install bundler gemrat

In this project, we need to use several dependencies. These dependencies are stored into a Gemfile. We will fill this file using Gemrat that we have previously install:

    $ gemrat jekyll jekyll-minibundle rake html-proofer

In the next section, we will cover each dependency and you will have a quick understanding when we start using them.

Unfortunately, the above command is to enough to install the dependencies. Indeed, we have just indicated that this project needs them. In order to run the installation, you will need to run:

    $ bundle install

After that, all dependencies we need for this tutorial will be available in our project.

## Asset fingerprint

The principle of a cache is to keep a copy of some file on the browser storage. Especially, we want to cache static files that do not change often and the idea is to serve these files quickly. Unfortunately, these files will not be updated any more if we set the expiration date too far. On the other hand, by setting a too close expiration date, the gain of browser caching will be limited.

The solution is to implement an asset fingerprint and it is simply the best of both worlds. It controls caching of static resources. Indeed, we will add fingerprints in the URLs for static content. So, we will keep the same fingerprint until the file is changed.

### Jekyll minibundle plugin

The reason we use use *jekyll minibundle* is that it provides an asset fingerprint. *jekyllminibundle* also implements an asset bundle but in this article, we will not use it.

After installing dependencies, we need to configure our Jekyll in order to load *jekyll minibundle*. Open *_config.yml* and add this following code at the end:

{% highlight yml %}
gems: [jekyll/minibundle]
{% endhighlight %}

Then, find all the location in your project to add the fingerprint. For example, in your HTML file, you can now put *ministamp* tag:

{% highlight html %}
{% raw %}
<link rel="stylesheet" href="{{ site.baseurl }}{% ministamp _assets/css/style.css assets/css/style.css %}">
{% endraw %}
{% endhighlight %}

Of course, you can also add a fingerprint for your JavaScript files.

## Rake task runner

For those who do not know what is a task runner, it is a tool performing a repetitive task. Here, we will implement tasks into the Rakefile in order to build your Jekyll site, test if your generated files are valid and deploy to GitHub pages.

### Build your Jekyll

### Test your Jekyll

### Deploy your Jekyll

## Conclusion
