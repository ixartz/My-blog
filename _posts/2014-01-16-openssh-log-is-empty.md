---
title: '[Quick tip] OpenSSH log is empty'
date: 2014-01-16 16:09:59
categories:
  - SSH
tags:
  - Log
  - Quick tips
  - SSH
---
Lately, I had some trouble with OpenSSH log (by default on Debian, the file is located at */var/log/auth.log*). My problem is that the log file is empty. For example, it is impossible to set up a system, which prevents Brute Force attacks (to prevent this, we need to look into the log). The first thing I did is to restart the SSH server by launching this command:

{% highlight sh %}
/etc/init.d/ssh restart
{% endhighlight %}

Unfortunately, it does not work. So, I tried to find the source of the problem in the configuration file. Although I never need to configure OpenSSH to activate log file (by default, OpenSSH writes automatically into a log file), I checked if all variables are correctly configured. In */etc/ssh/sshd_config* file, two variable are useful for us:

{% highlight sh %}
SyslogFacility AUTH
LogLevel INFO
{% endhighlight %}

On Internet, we can find a solution that replace *LogLevel INFO* by *LogLevel VERBOSE*:

{% highlight sh %}
SyslogFacility AUTH
LogLevel VERBOSE
{% endhighlight %}

After replacing and restarting the SSH server, nothing appeared in the log file and it was still blank.

## The solution

As often, it is a file ownership problem: OpenSSH uses syslog protocol to handle log information. A solution is to restart rsyslog daemon with root privilege:

{% highlight sh %}
sudo /etc/init.d/rsyslog restart
{% endhighlight %}

I wrote this post because on the one hand, I had this problem several times. So, I will not forget the solution anymore (I have a short memory) and on the other hand, I hope this post will help someone in the same situation as me.
