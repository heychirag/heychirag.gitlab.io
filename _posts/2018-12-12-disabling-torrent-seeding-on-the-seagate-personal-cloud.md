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
{: .text-justify}

Here is how the stock download manager works. It has two queues, one for active and one for inactive torrents. Depending on the settings, few torrents are moved to the active-queue initially while the rest must remain in the inactive-queue to wait for active torrents to complete. As soon as a torrent from the active-queue is marked completed, the next torrent waiting in line in the inactive-queue is moved to the active-queue. The problem? Torrents are never marked completed. Why? This is because when a torrent is done downloading, it starts to seed itself. So in theory, few torrents would seed forever while the rest would never get a chance to download. Unfair right? Hence the only possible solution was to find a way to limit the seed time of torrents, or maybe to stop them from seeding altogether.
{: .text-justify}

Everything is possible if you have an SSH and root access to a machine. (Click [here]({{ site.url }}/seagate-personal-cloud-hack-activating-ssh/) if you want to activate SSH on your Personal Cloud). Confident that some generic torrent client was being used at the backend, I went ahead and dug into the NAS OS's filesystem. I eventually found Transmission and knowing that making some necessary changes to its config file will do the trick, it worked!
{: .text-justify}

Go ahead and punch in the following commands in your shell to tame your Personal Cloud. But first, you need to go root:
{: .text-justify}

{% highlight shell %}
chirag@PersonalCloud:~$ sudo su
{% endhighlight %}

Enter the same password you used to SSH into the machine. If it throws up an error, you probably aren't an admin. Run this to know who is:
{: .text-justify}

{% highlight shell %}
chirag@PersonalCloud:~$ grep nas_admins /etc/group
{% endhighlight %}

Once you are root (own the responsibility!), open the config file for editing:
{: .text-justify}

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

Write/Exit the editor and restart the download manager from the NAS OS management interface.
{: .text-justify}

This essentially sets your seeding ratio to zero. This means torrents will doze off when done downloading. You should consider keeping this ratio to anything above zero as everyone in the torrenting world expects you to pay back your torrenting karmas.
{: .text-justify}

Cheers!
<div class="breaker"></div>
