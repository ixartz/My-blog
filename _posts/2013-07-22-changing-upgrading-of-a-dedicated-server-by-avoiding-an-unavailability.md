---
title: Changing / upgrading of a dedicated server by avoiding an unavailability
date: 2013-07-22 14:54:40
categories:
  - High availability
tags:
  - Bind
  - Exim
  - IP fail-over
  - Sendmail
  - Server
  - Virtual IP
---
Lately, I spend my time setting up a new dedicated server after receiving a mail from my hosting company. Indeed, my contract will be interrupted. So, I have no choice to change my server. In order to have the least impact for my websites hosted on old server, the best solution is to use a virtual IP (some web hosting company call an IP fail-over).

## Set up a virtual IP on Debian

######*I suppose that your new server is already set up*######

A virtual IP is very useful in several cases. For example, it can be used in high availability network. Here, we use a virtual IP for moving one server to another in few seconds.

Firstly, it is necessary to create a virtual network interface on the two servers. I will talk about how to do on Debian but you will find easily for other distributions on Internet.

In your */etc/network/interfaces* file, we need to add these two lines:

{% highlight sh %}
post-up /sbin/ifconfig eth0:1 (new IP) netmask 255.255.255.255 broadcast (new IP)
post-down /sbin/ifconfig eth0:1 down
{% endhighlight %}{: .small}

You need to do twice: one time with your old server and another time with your new server. So, */etc/network/interfaces* file on your two servers should contain following:

{% highlight sh %}
auto eth0
iface eth0 inet static
address xxx.xxx.xxx.xxx
netmask 255.255.255.0
broadcast xxx.xxx.xxx.255
network xxx.xxx.xxx.0
gateway xxx.xxx.xxx.254
post-up /sbin/ifconfig eth0:0 (new IP) netmask 255.255.255.255 broadcast (new IP)
post-down /sbin/ifconfig eth0:0 down
{% endhighlight %}{: .small}

Then, you just have to restart the network interface:

{% highlight sh %}
/etc/init.d/networking restart
{% endhighlight %}

## Deal with DNS servers

At the first time, the virtual IP must point toward the old server because domains still use the old IP. So, we need to point all website domains to the new virtual IP.

For example, a record for a domain using Bind9 located at */etc/bind/zones/mydomain.com.db*, you need to put your virtual IP at the line containing the current origin (the line containing '@' character):

{% highlight sh %}
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     root.mydomain.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
; ...
@       IN      A       (your virtual IP)
; ...
{% endhighlight %}

As usual, DNS propagation takes around 24/48 hours.

After waiting 2 days, it is quite straightforward to change server without any issues or any unavailability. Indeed, we just have to point the virtual IP from the old server to the new server.

Great! In few seconds, we can access to your website now hosted on your new server. Unfortunately, there is a very big issue regarding sending of mail from your new server. Especially, this issue is due to hotmail.com spam filter that considers our mails as spam. Frequently (not to say always), when you acquire a new IP, Hotmail treats your mails as unsafe. So, I suggest you to contact Hotmail to reinstate your virtual IP (it might last several days). So, do not hesitate to reinstate before moving to the new server.

## Sending mails with a virtual IP

Of course, I had to install a Mail Transfer Agent (MTA) and I chose Exim4 because I had already a little experience (I used Exim on my old server). This part is optional because if you does not keep your virtual IP. So, if you does not continue using your virtual IP, you need to change again your DNS server and you need to point all domains to your new server IP.

### Exim4, the wrong idea

I spent several nights to find how to send an email through an virtual IP with Exim. Unfortunately, I find anything to solve this problem.

######*I will anyway describe installation and configuration steps*######

Installing Exim4 is quite straightforward:

{% highlight sh %}
apt-get install exim4
{% endhighlight %}

To configure Exim4, it just enough to run the command and follow instructions:

{% highlight sh %}
dpkg-reconfigure exim4-config
{% endhighlight %}

Do not forget to restart your HTTP server if your websites need to send emails. For example, with an Apache server on Debian distribution, execute the following command:

{% highlight sh %}
/etc/init.d/apache2 restart
{% endhighlight %}

Now, you can send a mail but not from your virtual IP.

###Sendmail, the Messiah###

Firstly, we need to install Sendmail using a package manager:

{% highlight sh %}
apt-get install sendmail
{% endhighlight %}

To locate where is the configuration file, type/copy-paste in your terminal:

{% highlight sh %}
sendmail -d0.11
{% endhighlight %}

And find the following line:

{% highlight sh %}
Conf file:    (file location)     (selected)
{% endhighlight %}

For me, the configuration file is */etc/mail/submit.cf*. So, in *submit.cf*, uncomment this line (remove *#*):

{% highlight sh %}
#O ClientPortOptions=Family=inet, Address=0.0.0.0
{% endhighlight %}

And, change *0.0.0.0* to your virtual IP.

Restart Apache and Sendmail by executing these commands:

{% highlight sh %}
/etc/init.d/sendmail r
/etc/init.d/apache2 restart
{% endhighlight %}

Voilà, every mail that will send from your new server will use your virtual IP.

## Conclusion

In conclusion, it is very easy to have almost no impact when upgrading or updating a server with a virtual IP. Nevertheless, it is necessary to prepare each steps. Especially, dealing with email services.

*PS: Hotmail spam detection is very picky! However, I have no choice to deal with because 40% of my members use Hotmail to sign up on my websites.*
