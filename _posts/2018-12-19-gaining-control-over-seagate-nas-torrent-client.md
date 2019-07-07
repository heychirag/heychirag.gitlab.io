---
title: Gaining control over Seagate Personal Cloud's torrent client
layout: post
date: '2018-12-19 20:38:43 +0530'
category: blog
tag:
- seagate personal cloud
- hacks
keywords: connect, seagate, personal, cloud, torrent, client, external, web, interface
author: heychirag
---

For some reason, I am just not able to get enough of finding and trying out different ways to take control over my NAS's torrent client.

After realizing my NAS uses transmission as its default torrent client, it didn't take me long to check if I had access to its web interface. The web interface is similar to the GUI of a torrent client. No doubt, it is the best way to manage torrents as almost every possible setting can be adjusted through it. So I instantly jumped over to `http://nas.lan:9091` (9091 is the port transmission runs on), but unfortunately, a `403 Forbidden` message greeted me. Quite obviously, the Seagate NAS OS developers had to design everything so meticulously and fix all vulnerabilities to block my access to transmission's web interface. However, upon Googling the issue, I found that there was an easy fix to this problem; which was to add the following rule to the config file (check [this]({{ site.url }}/disabling-torrent-seeding-on-the-seagate-personal-cloud/) post if you would like to know where to find the config file):

{% highlight json %}
{% raw %}
{
    "rpc-whitelist": "127.0.0.1,10.0.0.*",
}
{% endraw %}
{% endhighlight %}

`10.0.0.*` whitelists all the IP's present on my home network which enables me to access the web interface from all the devices at my home. However, you may want to prevent unauthorized access. In that case, you can whitelist only some IPs or even enable `rpc-authentication-required` by specifying an `rpc-username` as well as an `rpc-password`. Do keep in mind that enabling this will screw up Seagate's stock Download Manager, but If you do not intend to use it anyway, then no harm shall be caused.

But wait, if we now try and open the web interface, we see the following message.

![Missing web interface files]({{ site.url }}/assets/images/web-interface-files-missing.png)
<!--figcaption class="caption">Transmission web interface files are missing on the NAS OS</figcaptio-->

The developers decided to omit the web interface files to keep the OS lite. And unfortunately, there's no easy way to install the web interface on your own due to a missing package manager.

The good news is transmission daemon is now listening to us over the 9091 port. So we can now go ahead and try connecting to it using an external transmission client.

Few clients are already available on the Play Store. I have tested two of them and they work really well (screenshots below).

<div style="width:100%;margin:6rem 0 6rem 0%;" class="side-by-side">
    <div class="toleft">
        <img class="image" src="{{ site.url }}/assets/images/remote-transmission.jpg" alt="Remote Transmission">
        <figcaption class="caption">Remote Transmission</figcaption>
    </div>

    <div class="toright">
        <img class="image" src="{{ site.url }}/assets/images/transmission-remote.jpg" alt="Transmission Remote">
        <figcaption class="caption">Transmission Remote</figcaption>
    </div>
</div>


If you are looking for a desktop client, check out [this](https://github.com/transmission-remote-gui/transgui) GitHub repository that maintains transmission clients for Linux, Windows and macOS.

_Bonus tip:_ If your ISP provides a static public IP, you can add that to the whitelist. Further, by enabling port forward on your router, you can connect to your NAS torrent client from anywhere in the world.

Cheers!
<div class="breaker"></div>
