---
title: Disabling torrent seeding on the Seagate Personal Cloud
layout: post
date: '2018-12-12 02:32:08 +0530'
category: blog
tag:
- seagate personal cloud
- hacks
keywords: root, access, disable, deactivate, torrent, seeding, seagate, personal, cloud
author: heychirag
---

One of the amazing features that the Seagate Personal Cloud equipped with is its ability to download torrents directly on the NAS itself. But also, not one of the great features is the lack of control Seagate gave user over the torrents.

Here is how the stock download manager works. There are two queues, one for active and one for inactive torrents. The active-queue is a special one because only a limited number of torrents can stay in it at any given time. For the download manager to move a new torrent from the inactive-queue to the active-queue, a torrent from the active-queue must be stopped when it's completed. Why doesn't the torrents stop automatically when completed? This is because when a torrent is done downloading, it starts seeding itself without realizing new torrents are waiting in the inactive-queue to be moved to the much-coveted active-queue. In theory, some torrents would seed forever while new torrents would never get a chance to download. Unfair right? The only possible solution to solve this problem was to find a way to limit the seed time of torrents or maybe stopping them from seeding altogether.

Everything is possible if you have an SSH and root access to a machine. (Click [here]({{ site.url }}/seagate-personal-cloud-hack-activating-ssh/) if you'd like to learn to activate SSH on the Seagate Personal Cloud). Confident that some generic torrent client was being used at the backend, I went ahead and dug into the NAS OS's filesystem. I eventually found Transmission and knowing that making some necessary changes to its config file will do the tricks, I made it work.

Go ahead and punch in the following commands in your shell to do the same with your Personal cloud. But first, go root:

{% highlight shell %}
chirag@PersonalCloud:~$ sudo su
{% endhighlight %}

Enter the same password you used to SSH into the machine. If it throws up an error, you probably aren't an admin then. Run this to know who is:

{% highlight shell %}
chirag@PersonalCloud:~$ grep nas_admins /etc/group
{% endhighlight %}

Once you are root (own the responsibility!), open the config file for editing:

{% highlight shell %}
root@PersonalCloud:~# vi /root/.config/transmission-daemon/settings.json
{% endhighlight %}

Make the following changes to the JSON:

{% highlight json %}
{% raw %}
{
    ...
    "ratio-limit": 0,
    "ratio-limit-enabled": true,
    ...
}
{% endraw %}
{% endhighlight %}

write/exit the config and restart the download manager from the Personal Cloud management interface.

This essentially sets your torrent seeding ratio to zero. So no more seeding and going to sleep when done downloading. You may consider setting this ratio to something above zero as everyone in the torrenting world expects you to pay back your torrenting karmas.

Cheers!
<div class="breaker"></div>
