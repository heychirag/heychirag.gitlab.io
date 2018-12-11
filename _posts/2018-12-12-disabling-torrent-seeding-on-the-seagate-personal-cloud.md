---
title: Disabling torrent seeding on the Seagate Personal Cloud
layout: post
date: '2018-12-12 02:32:08 +0530'
category: blog
tag:
- seagate personal cloud
- hacks
author: heychirag
---

One of the amazing features that the Seagate Personal Cloud equipped with is its ability to download torrents directly on the NAS itself. But also, not one of the great features is the lack of control Seagate gave to the user over the torrents.

One of the biggest issues that I faced with the stock download manager was that once a bunch of torrents are in the queue, completed torrents start seeding instead of new ones downloading. This was because seeding torrents are categorized as active, and since there is a limit to how many torrents can stay active at a time, torrents next in the queue are not switched to the active state. So in theory, new torrents would never get downloaded as seeding torrent occupy the active quota. One possible solution was to increase the number of simultaneous downloads, but that isn't too practical provided the tiny amount of RAM and a low powered processor that ships with the Personal Cloud. The only solution was to find a way to stop the automatic torrent seeding!

Everything is possible if you have SSH and root access to any machine. (Click [here]({{ site.url }}/seagate-personal-cloud-hack-activating-ssh/) if you'd like to learn to activate SSH on the Seagate Personal Cloud). I went ahead and found the config file of Transmission(the torrent client) and changed some variables. Voila! The problem was solved.

To achieve the same on your Personal cloud, you will be needing root access to perform the changes to the Transmission config file. Run the following command to go root:

    [chirag@PersonalCloud ~]$ sudo su

Enter the same password you used to SSH into the Personal Cloud. If it throws up an error, then you probably aren't the admin. Run this command to know who is:

    [chirag@PersonalCloud ~]$ grep nas_admins /etc/group

Once you are root (own the responsibility), open the config file for editing using:

    [root@PersonalCloud ~]# vi /root/.config/transmission-daemon/settings.json

Make the following changes to the JSON:

    {
      ...
      "ratio-limit": 0,
      "ratio-limit-enabled": true,
      ...
    }

write/exit the config and restart the download manager from the Personal Cloud management interface.

What this does is sets the seeding ratio to zero. And this means that torrents will automatically stop once they have finished downloading. You can also set this ratio to any positive real number as everyone in the torrenting world expects you to be somewhat generous.

Cheers!
