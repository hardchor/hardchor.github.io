---
id: 292
title: Depending on packages without composer.json in Composer (PHP dependency manager)
author: Burgi
layout: post
guid: http://burgiblog.com/?p=292
permalink: /2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/
categories:
  - Development
  - symfony
tags:
  - Composer
  - Packagist
  - PHP
  - symfony
  - Symfony2
---


<a href="http://getcomposer.org" target="_blank">Composer</a> is a great way to manage your PHP dependencies. The <a href="http://getcomposer.org/doc" target="_blank">documentation</a> is still a bit scarce, so you could imagine that it took a while to find out how to add an external dependency (in my case from github) which doesn&#8217;t have a composer.json file and is not registered on packagist. There are <a href="http://speakerdeck.com/u/naderman/p/composer-php-user-group-karlsruhe" target="_blank">different</a> <a href="http://getcomposer.org/doc/05-repositories.md" target="_blank">solutions</a>, but Composer seems to be under active development and none worked for me.

<!--more-->

So &#8211; in my project&#8217;s *composer.json* &#8211; this is how I&#8217;m loading the FOSFacebookBundle:

1. Define it as an external repository:

<pre class="brush: javascript; gutter: true">"repositories": {
        "symfony-unofficial": {
            "type": "package",
            "package": {
                "name": "fos/facebookbundle",
                "version": "2.0",
                "source": {
                    "url": "git://github.com/FriendsOfSymfony/FOSFacebookBundle.git",
                    "type": "git",
                    "reference": "origin/2.0"
                }
            }
        }
    }</pre>

2. Add it to your project&#8217;s dependencies:

<pre class="brush: javascript; gutter: true">"require": {
        ...
        "fos/facebookbundle":             "2.0.*"
    }</pre>

3. And register the (PSR-0 conform) namespace with the autoloader:

<pre class="brush: javascript; gutter: true">"autoload": {
        "psr-0": {
            "MyBundleNamespace": "src/",
            "FOS": "vendor/"
        }
    }</pre>

Et voila! By the way, my *composer.json* would look like this:

<pre class="brush: javascript; gutter: true">{
    "name": "My app",
    "description": "My Facebook App",
    "repositories": {
        "symfony-unofficial": {
            "type": "package",
            "package": {
                "name": "fos/facebookbundle",
                "version": "2.0",
                "source": {
                    "url": "git://github.com/FriendsOfSymfony/FOSFacebookBundle.git",
                    "type": "git",
                    "reference": "origin/2.0"
                }
            }
        }
    },
    "require": {
        "php":              "&gt;=5.3.2",
        "symfony/symfony":  "&gt;=2.0.10,&lt;2.1.0-dev",
        "doctrine/orm":     "&gt;=2.1.0,&lt;2.2.0-dev",
        "twig/extensions":  "*",

        "symfony/assetic-bundle":         "2.0.*",
        "sensio/generator-bundle":        "2.0.*",
        "sensio/framework-extra-bundle":  "2.0.*",
        "sensio/distribution-bundle":     "2.0.*",
        "jms/security-extra-bundle":      "1.0.*",
        "fos/facebookbundle":             "2.0.*"
    },
    "autoload": {
        "psr-0": {
            "MyBundleNamespace": "src/",
            "FOS": "vendor/"
        }
    }
}</pre>

