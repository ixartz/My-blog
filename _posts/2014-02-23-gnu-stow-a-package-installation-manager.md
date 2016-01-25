---
title: GNU Stow, a package installation manager
date: 2014-02-23 02:10:25
categories:
  - Tool
tags:
  - GNU
  - Installation
  - Manager
  - Package
  - Tool
---
Currently, I work on a compiler and I use <a title="Bison - GNU parser generator" href="http://www.gnu.org/software/bison/" target="_blank">GNU Bison</a> to generate a parser. It is very painful to manage the different versions of Bison (I switch between several versions). Not long ago, I discovered a great tool named <a title="GNU Stow" href="http://www.gnu.org/software/stow/" target="_blank">GNU Stow</a> that simplifies managing different versions of a software package.

### What is the usefulness to use Stow?

It is true that there are already package manager but Stow is very interesting to manage packages, which need to be installed by hand (for example, unofficial packages).

## Installation

It is very easy to install Stow on Linux (and on Mac). Indeed, Stow is included in every major Linux repository. For example, on Ubuntu, you need to run this line:

{% highlight sh %}
sudo apt-get install stow
{% endhighlight %}

After that, you have to create a folder named *stow* at */usr/local/*:

{% highlight sh %}
sudo mkdir /usr/local/stow
{% endhighlight %}

## Stow in action

###Installing a package with Stow###

Stow requires that you change the default installation path by */usr/local/stow*. With a *make install*, you can specify an argument (*prefix=*) that indicates where the package will be installed:

{% highlight sh %}
make && sudo make install prefix=/usr/local/stow/(package-name)
{% endhighlight %}

###Enabling (Stowing) a package###

After installing, you need to enable the package by running this command:

{% highlight sh %}
cd /usr/local/stow && sudo stow (package-name)
{% endhighlight %}

###Disabling (Unstowing) a package###

You just need to add *-D* argument in Stow command to disable a package:

{% highlight sh %}
cd /usr/local/stow && sudo stow -D (package-name)
{% endhighlight %}

### Uninstalling a package

Removing */usr/local/stow/(**package-name**)* folder is a good to uninstall definitively or to upgrade a package. Before doing this, you need to remove symbolic links created by Stow:

{% highlight sh %}
cd /usr/local/stow && sudo stow -D (package-name)
sudo rm -R /usr/local/stow/(package-name)
{% endhighlight %}

## Conclusion

I hope I could help people who had issues to manage different application of a software. Indeed, Stow is certainly a great tool to handle this.
