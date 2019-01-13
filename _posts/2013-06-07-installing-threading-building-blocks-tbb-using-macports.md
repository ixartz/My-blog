---
title: Installing Threading Building Blocks (TBB) using MacPorts
date: 2013-06-07 23:46:51
description: How to install TBB on Mac with MacPorts. Follow this guide for instructions on setting up Threading Building Blocks.
categories:
  - C++
tags:
  - Library
  - MacPorts
  - Parallelism
  - Quick tips
  - Thread
---
![C++ template library for task parallelism]({{ site.baseurl }}assets/images/posts/tbb-large-product-plain.png){: .alignright}Yesterday, I had to use Threading Building Blocks (TBB), an Intel C++ template library to take advantage of multicore processors. But, I had some troubles to create an executable and I had difficulty to find the solution on Internet. So, I write this thread in order to help futur users of TBB on Mac.

## Download and installation

After installing TBB library and its dependencies with the following command:

{% highlight sh %}
sudo port install tbb
{% endhighlight %}

I got no error messages and I thought everything worked. Indeed, g++ has not problems to find TBB headers. I could compile this code without trouble:

{% highlight c++ %}
#include <iostream>
#include "tbb/tick_count.h"

int main()
{
  tbb::tick_count t0 = tbb::tick_count::now();

  // Do something very long

  tbb::tick_count t1 = tbb::tick_count::now();
  printf("time = %g seconds\n", (t1 - t0).seconds());

  return 0;
}
{% endhighlight %}

{% highlight sh %}
g++ -W -Wall -Wextra -Werror -pedantic -std=c++11 main.cc
{% endhighlight %}

### Compilation errors

![A step-by-step guide for TBB]({{ site.baseurl }}assets/images/posts/error.png){: .alignleft}Unfortunately, there are some errors which appears when we add **-ltbb** flag in previous command:

{% highlight c++ %}
#include "tbb/task_scheduler_init.h"

int main()
{
  tbb::task_scheduler_init init;

  // ...

  return 0;
}
{% endhighlight %}

{% highlight sh %}
g++ -W -Wall -Wextra -Werror -pedantic -std=c++11 -ltbb main.cc
{% endhighlight %}

{% highlight sh %}
Undefined symbols for architecture x86_64:
  "tbb::task_scheduler_init::initialize(int, unsigned long)", referenced from:
      tbb::task_scheduler_init::task_scheduler_init(int, unsigned long) in test.o
  "tbb::task_scheduler_init::terminate()", referenced from:
      tbb::task_scheduler_init::~task_scheduler_init() in test.o
ld: symbol(s) not found for architecture x86_64
collect2: error: ld returned 1 exit status
{% endhighlight %}

This flag allows to load the library files (not only headers). So, if we can not compile with this flag, TBB would be very limited.

## The solution

In order to fix this error, type the following command:

{% highlight sh %}
source /opt/local/bin/tbbvars.sh
{% endhighlight %}

Now, you can execute the compilation command with **-ltbb** flag. I hope it helps you to save a lot of time. If you still have some error, leave a comment and I will try to help you. In few weeks, I will probably talk about TBB more deeply that is to say how I used it in video processing.
