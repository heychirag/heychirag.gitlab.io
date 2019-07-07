---
title: Activating SSH on the Seagate Personal Cloud
layout: post
date: '2018-12-01 21:10:36 +0530'
category: blog
tag:
- seagate personal cloud
- hacks
keywords: enable, ssh, access, seagate, personal, cloud
author: heychirag
---

If you are a tinkerer like me and you own a Seagate Personal Cloud, you probably would have wanted to be able to SSH into your new NAS. But going down the services page in the Device Manager, you may have found an option to enable SFTP but not the SSH. Don't worry, you can enable SSH on your Seagate Personal Cloud with this simple hack.

Head over to the following address replacing the angular brackets with your NAS IP.

    http://<SEAGATE PERSONAL CLOUD IP>/?appdev=1

Going over to the services page in the Device Manager, you'll now be able to see an option to toggle the SSH Access.

![Enable SSH Access]({{ site.url }}/assets/images/seagate-ssh-access.png)

If you are already using the SFTP service, you'll need to switch it off since otherwise, it'll result in a port clash (both run on port 22). You'll still be able to use the SFTP service with the SSH Access turned on since SFTP is an inherent feature of SSH but not the other way around.

Cheers!
<div class="breaker"></div>

