---
title: Create a session timeout in Symfony 1.4
date: 2013-02-07 18:12:30
categories:
  - Symfony
tags:
  - Garbage Collector
  - PHP
  - Session
  - Symfony
  - Timeout
---
![The subject of this post]({{ site.baseurl }}assets/images/posts/hourglass.png){: .alignleft}I will begin by writing my first technical post on Symfony 1.4. I know 1.4 is obsolete (Symfony 2 have released and since November 2012, Symfony 1.x is not maintained anymore) but I am sure there is a lot of websites are currently running on Symfony 1.4. This post is my first and also the last one on 1.4 unless it has a huge success. I can still dream ;)

## Session timeout only with PHP

### Client side

![Users around the world]({{ site.baseurl }}assets/images/posts/client.png){: .alignright}In *php.ini* file, we can define a lifetime for sessions using cookies (the directive is named *session.cookie_lifetime*). So, we let the browsers manage the end of a session. Of course, browsers can delete cookie that stores session information when they want and not when we want. Moreover, today, browsers delete session cookie only when users close the window. So, it is a problem if you want to create a session timeout after some user's inactivity time.

### Server side

![A server which deliver pages]({{ site.baseurl }}assets/images/posts/server.png){: .alignright}The other solution to create a session limited in time is to use another PHP directives, which change the session storage on server. Managing session timeout on server side has advantage of controlling session lifetime with *session.gc_maxlifetime*. Indeed, we no longer have to wait goodwill of browsers. By default, *session.gc_maxlifetime* is equal to 1 440 that is to say 24 minutes.

![The logo of PHP]({{ site.baseurl }}assets/images/posts/php.gif){: .alignleft}However, this does not mean that sessions are deleted after 1 440 seconds. In fact, *session.gc_probability* and *session.gc_divisor* are two important directives for the Garbage Collector (which deals with cleaning old session). The Garbage Collector is called with a probability of *session.gc_probability* divided by *session.gc_divisor*. Without changing anything, *session.gc_probability* (respectively *session.gc_divisor*) is equal to 1 (respectively equal to 100). So, there is 1/100% (1%) chance that the Garbage Collector cleans old session.

### Symfony 1.4

![The logo of Symfony 1.x]({{ site.baseurl }}assets/images/posts/symfony1.gif){: .alignright}This second solution has a disadvantage if we want all sessions to be deleted after a specific time of inactivity from the user. Of course, theoretically, we can set *session.gc_probability* and *session.gc_divisor* to 1. Nevertheless, launch the Garbage Collector on each request use resources unnecessarily.

To solve this problem effectively, we will use Symfony settings. In *config/factories.yml*, we can add this few lines:

{% highlight yaml %}
all:
  user:
    class: myUser
    param:
      timeout: 1200
{% endhighlight %}

You can replace 1 200 by everything you want. This number represents session lifetime in seconds. Unlike the two previous solutions, we are sure that users will be logout after a precise time of inactivity. Indeed, after 1 200 seconds of inactivity, *isAuthenticated()* will return *false* and we use this function to check if the user is connected or not. The *isAuthenticated()* function is implemented in *sfUser* class. So, inside the *view* layer, we use *$sf_user->isAuthenticated()* and in *controller*, we call *$this->getUser()->isAuthenticated()*.

## Conclusion

![The logo of Symfony 2.x]({{ site.baseurl }}assets/images/posts/symfony2.png){: .alignright}Through this first post, I hope that you have learned something and this post will be useful to you.
