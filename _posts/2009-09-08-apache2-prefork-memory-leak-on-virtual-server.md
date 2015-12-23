---
id: 163
title: Apache2-prefork memory leak on virtual server
author: Burgi
layout: post
guid: http://burgiblog.com/?p=163
permalink: /2009/09/08/apache2-prefork-memory-leak-on-virtual-server/
categories:
  - openSuse 11.1
tags:
  - 10.3
  - 11.1
  - 2.2.10
  - 2.2.13
  - apache
  - apache2
  - apache2-prefork
  - fork
  - memory leak
  - openSuse
  - prefork
---


Out of the blue, our **Vserver** (**openSuse 11.1**) suddenly went mad. This all began with it denying SSH access. Then, after forcing a restart through the rescue system <a title="Strato" href="http://strato.de" target="_blank">Strato</a> provides, I could log on via shell again. Unfortunately, after a while, openSuse kept quitting any prompt with  
`-bash: fork: Not enough memory available`  
<!--more-->

After a restart, watching `top` for a while didn&#8217;t help too much, and neither did `ps aux`, besides noticing that the memory kept rising. Unfortunately, searching google didn&#8217;t help too much either. We couldn&#8217;t narrow down the problem. So we kept check ing the main components, starting with pure-ftpd, mysql, and finally apache2, which lead us to our solution. The error logs looked a bit suspicious:  
` *******  /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)<br />
******* /usr/sbin/cron[20026]: (CRON) error (can't fork)`

We then had a closer look at how apache forks. Since we decided to not use the openSuse 10.3/Plesk option Strato provided us, we formerly installed openSuse 11.1 as our OS of choice and rebuilt apache2 etc from scratch, not bothering too much about the configuration, that, by default, is not fit for a resource-poor system like a virtual server.

To add to that,** Apache2-prefork 2.2.10** onwards seems to have a minor **memory leak**, causing it to use more and more memory the longer it runs (or at least that seems to be the case with our configuration). So what we did was going into the apache2 server config (in our case under /etc/apache2/server-tuning.conf) and changed  
`<br />
StartServers         5<br />
MinSpareServers      5<br />
MaxSpareServers     15<br />
ServerLimit        150<br />
MaxClients         150<br />
MaxRequestsPerChild  10000<br />
`  
to  
`<br />
StartServers         3<br />
MinSpareServers      3<br />
MaxSpareServers     15<br />
ServerLimit        150<br />
MaxClients         150<br />
MaxRequestsPerChild  10`

Et voila, it works! You might want to adjust those settings to your own requirements, but especially MaxRequestsPerChild should yield a much lower value than 10000 since that&#8217;s the number after how many requests apache will kill a fork and hence, prevent it from using too much memory.

I hope this was any help to you.

