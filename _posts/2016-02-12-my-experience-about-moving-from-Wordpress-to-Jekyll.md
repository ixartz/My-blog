---
title: 'My experience about moving from Wordpress to Jekyll'
date: 2016-02-12 17:41:56
description: How to convert your Wordpress to a Jekyll blog. A guided tutorial to install Jekyll from a Wordpress.
categories:
  - Jekyll
tags:
  - Wordpress
  - GitHub
  - Disqus
  - migration
---
## Why I migrate from Wordpress to Jekyll

This post is my first one I publish on Jekyll. Recently, I made the choice to upgrade from Wordpress to Jekyll. Below, I list the advantages and disadvantages about replacing Wordpress by Jekyll:

### The pros

* Constantly we need to update Wordpress
* Security issues in Wordpress
* Host your Jekyll on GitHub for free with your own domain name
* You can use a versoning software to track your modications in Jekyll
* Use markdown for Jekyll instead of a WYSIWYG editor

### The cons

* Jekyll is not user friendly for people who do not know git or Markdown
* Dynamic part in Jekyll is very limited
* Number of Jekyll themes are very limited

The main reason pushes me to use Jekyll is that Wordpress is hard to maintain (security, plugins, etc.). My Wordpress was also hosted on my own server that was also painful to configure (SSH, database, FTP, etc.). So, I wanted something easy and Jekyll was the perfect solution.

![Jekyll, blogging system]({{ site.baseurl }}assets/images/posts/jekyll-logo.png){: .alignright}Indeed, Jekyll is a blogging framework who generates static website. We use Jekyll through a package manager, this means it is straightforward to keep Jekyll up to date. And, the fact that Jekyll generates static website, we have less security issue. Moreover, you can host your Jekyll blog on GitHub pages. With Jekyll, you can really focus on your contents instead of losing your time in configuration and maintenance.

## How to move from Wordpress to Jekyll

### Install and run Jekyll

If RubyGems is already installed on your machine, you only need four commands to launch Jekyll from scratch:

{% highlight sh %}
gem install jekyll
jekyll new my-blog-name
cd my-blog-name
jekyll serve
# You can open a web browser at this location: http://localhost:4000
{% endhighlight %}

### Personalize your Jekyll

You can easily customize your Jekyll by modifying *_config.yml* and HTML & CSS files. But if you are not a web designer, you can also find a theme on the Internet. Unfortunately, the number of Jekyll themes is very poor compared to the Wordpress ones.

My Jekyll blog is hosted on GitHub and if you want to do the same, you can <a href="https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/" target="_blank">set up a custom domain with GitHub Pages</a>.

### Put your comment system on Disqus

![Disqus comment system for Jekyll]({{ site.baseurl }}assets/images/posts/disqus-logo.png){: .alignleft}On Wordpress, I have already used <a href="https://disqus.com/" target="_blank">Disqus as comment system</a>. So, it was very easy to migrate the comments. The only thing you need to be careful is that your Jekyll permalink matches your Wordpress one. You have also other ways to migrate your comments and do not keep your permalink. But, I will not detail here.

For someone who did not use Disqus before, you can also import your Wordpress comments into Disqus system. And after that, you can migrate to Jekyll very easily.

### Import your content from Wordpress

There is a very nice Wordpress plugin named <a href="https://wordpress.org/plugins/jekyll-exporter/" target="_blank">Jekyll Exporter</a> that can convert your Wordpress to Jekyll compatible files (configuration file, posts in Markdown and images).

![Convert all Wordpress posts, pages and settings to Jekyll]({{ site.baseurl }}assets/images/posts/wordpress-to-jekyll.png){: .aligncenter}

Two important things you need to know about *Jekyll Exporter*:

* *Jekyll Exporter* will not convert everything to Markdown. When it cannot convert, the HTML tag will remain.
* You will probably have some issue with images. You can either update the image location in your posts or either put Jekyll image's location as same as the Wordpress one.

### Jekyll workflow

If you use Jekyll with GitHub pages, your workflow to write and publish will be very short:

{% highlight sh %}
vim _posts/new-post-subject.md
# Write something in your post (the most complicated part)
git add _posts/new-post-subject.md
git commit -m "Commit message"
# git commit will automatically publish your post on your Jekyll blog
{% endhighlight %}

## Conclusion

Jekyll is the perfect fit for someone who know git workflow and Markdown. You do not need to be a master of Markdown, but at least, you are not afraid to use a markup language such as HTML, XML, latex, etc. Markdown is just another markup language but it is a lightweight one.

You want to see the result? This blog runs on Jekyll and the code source is <a href="https://github.com/ixartz/My-blog" target="_blank">at this location</a>.
