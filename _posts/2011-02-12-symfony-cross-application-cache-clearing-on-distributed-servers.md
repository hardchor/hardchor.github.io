---
id: 252
title: 'Symfony: Cross-application cache clearing on distributed servers'
author: Burgi
excerpt: "I've finally published very first symfony plugin. This post introduces mbpDistributedCachePlugin, a plugin that I've written during my time at MadeByPi to synchronise clearing the cache in a multi-server setup. This is especially relevant to highly frequented sites, but can be used for single-server environments just as easy as it also provides functionality to clear parts of the cache across applications (e.g. from your backend to your frontend application)"
layout: post
guid: http://burgiblog.com/?p=252
permalink: /2011/02/12/symfony-cross-application-cache-clearing-on-distributed-servers/
categories:
  - Development
  - symfony
tags:
  - cache
  - cluster
  - multiple servers
  - PHP
  - plugin
  - symfony
  - symfony 1.4
---


<img class="alignright size-full wp-image-265" style="padding: 0 0 10px 10px;" title="cluster_icon" src="http://burgiblog.com/wp-content/uploads/2011/02/cluster_icon.jpg" alt="" width="128" height="128" />  
While the title might sound a tad complicated, the problem is pretty straight forward:

Your production environment consists of a **multi-server set-up** (e.g. a cluster, but works on a single server just as well) with a **centralised database** and you use **sfFileCache** as your caching strategy (because you don&#8217;t want to litter your database with cache entries).

So how would you clear parts or all of the cache? You can&#8217;t just use `symfony:cc` or the `sfViewCacheManager` because:

  * It won&#8217;t work across applications, e.g. when triggered from your backend application, you won&#8217;t be able to clear the cache on your frontend.
  * It will only clear the cache on your current server, while the other servers still contain the old cache version.

<!--more-->

  
Along comes our centralised database! The solution we came up with at <a title="Digital Agency in Leeds" href="http://www.madebypi.co.uk" target="_blank">work</a> was to write all cache clearing rules to the database and then run a cronjob every minute or so to process the pending rules. It already works really well on a highly frequented site we&#8217;ve released earlier this year and we&#8217;ve decided to package it into a symfony plugin (<a title="symfony plugin to clear the cache on multiple=" href="http://www.symfony-project.org/plugins/mbpDistributedCachePlugin" target="_blank">mbpDistributedCachePlugin</a>).

## How to install

You can install the latest stable version via

    symfony plugin:install mbpDistributedCachePlugin

or check out the latest version from

    svn co http//svn.symfony-project.com/plugins/mbpDistributedCachePlugin/trunk plugins/mbpDistributedCachePlugin

I&#8217;ve put detailed installation and usage instruction into the README file (also available <a href="http://www.symfony-project.org/plugins/mbpDistributedCachePlugin/1_0_0?tab=plugin_readme" target="_blank">here</a>). However, if you should have any questions or suggestions, leave me a comment and I&#8217;ll get back to you.

