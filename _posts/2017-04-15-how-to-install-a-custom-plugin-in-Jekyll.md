---
title: 'How to install a custom plugin in Jekyll'
date: 2017-04-15 23:18:24
description: Add an asset fingerprint for Jekyll
categories:
  - Jekyll
tags:
  - GitHub
  - Rake
  - Gemfile
  - Minify
  - Fingerprints
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

In this project, we need to use several dependencies. These dependencies are stored into a Gemfile. We will fill this file using Gemrat that we have previously installed along with Bundler:

    $ gemrat jekyll jekyll-minibundle rake html-proofer

In the next section, we will cover each dependency and you will have a quick understanding when we start using them.

Unfortunately, the above command is enough to install the dependencies. Indeed, we have just indicated that this project needs them. In order to run the installation, you will need to run:

    $ bundle install

After that, all dependencies we need for this tutorial will be available in our project.

## Asset fingerprint

The principle of a cache is to keep a copy of some file on the browser storage. Especially, we want to cache static files that do not change often and serve these files quickly. Unfortunately, these files will not be updated any more if we set the expiration date too far. On the other hand, by setting a too close expiration date, the gain of browser caching will be limited.

Implementing an asset fingerprint is a solution and it is simply the best of both worlds. It controls caching of static resources. Indeed, we will add fingerprints in the URLs for static content. So, we will keep the same fingerprint until the file is changed.

### Jekyll minibundle plugin

The reason we use *jekyll minibundle* is that it provides an asset fingerprint. *jekyll minibundle* also implements an asset bundle but in this article, we will not use it.

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

For those who do not know what is a task runner, it is a tool performing a repetitive task. The reason why we need Rake is because it is not possible to use a custom plugin on GitHub pages. It means we are not able to use Jekyll source files directly.

The solution is to implement tasks in the Rakefile in order to build your Jekyll site, test if your generated files are valid and deploy to GitHub pages.

### Create a Rakefile

First, we need to create a Rakefile in your project's root directory:

    $ touch Rakefile

Then, inside the Rakefile, add the following lines:

{% highlight ruby %}
require 'rake/clean'

CLEAN.include '_site'
{% endhighlight %}

By default, the generated files will be placed into *_site* folder. So, when we run a clean with rake, we want to remove the *_site* folder.

### Build your Jekyll

The first task we will implement is to build our project:

{% highlight ruby %}
namespace :jekyll do
  desc 'Compile your Jekyll source files'
  task build: :clean do
    sh %{jekyll build}
  end
end
{% endhighlight %}

Now, by running this command in your terminal:

    $ bundle exec rake jekyll:build

Rake will clean the project, and then it will generate your files into *_site* folder.

### Test your Jekyll

After building your Jekyll site, we want to make sure the output files are correctly generated. The next task is to validate our HTML files by using *html-proofer*:

{% highlight ruby %}
namespace :test do
  options = { :check_html => true }

  desc 'Test the generated files'
  task :html do
    HTMLProofer.check_directory('./_site', options).run
  end
end
{% endhighlight %}

You can now run this command:

    $ bundle exec rake test:html

It will print in your terminal all the errors in your project (related to the generated files).

### Deploy your Jekyll

The final task is to deploy your *_site* to GitHub pages:

{% highlight ruby %}
namespace :deploy do
  desc 'Deploy the generated files on GitHub'
  task :pages do
    repo = `git remote get-url origin`.tr("\n","")

    FileUtils.cd('_site', :verbose => true) do
      sh %{rm -rf .git}
      sh %{git init && git add .}
      sh %{git commit -m 'Deploy on GitHub pages'}
      sh %{git push -f #{repo} master:gh-pages}
    end
  end
end
{% endhighlight %}

The idea of deploy task is we create a new local git repository. It will add a new commit and it will push to the remote branch named gh-pages.

*Note: It is recommended to put _site folder in .gitignore file, we should avoid using Git subtree*

You will be able to type this line in your terminal:

    $ bundle exec rake deploy:pages

Then, open your favorite browser and go to your website. You will see your static resources with an asset fingerprint when you open your site's source. For example, something like:

{% highlight html %}
<link rel="stylesheet" href="/assets/css/style-de5e7cd8dfc06d18d371854de0e84c6c.css">
{% endhighlight %}

## Conclusion

We are at the end of this tutorial about using a custom plugin on Jekyll. We have also implemented some tasks with Rake to build, test and deploy your Jekyll easily. In the next article, we will set up a continuous integration and continuous deployment to do these tasks automatically for us.
