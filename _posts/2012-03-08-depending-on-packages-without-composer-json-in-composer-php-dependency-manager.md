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
<p class="wp-flattr-button">
  <a class="FlattrButton" style="display:none;" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/" title=" Depending on packages without composer.json in Composer (PHP dependency manager)" rev="flattr;uid:BurkhardR;language:en_GB;category:audio;tags:Composer,Packagist,PHP,symfony,Symfony2,blog;button:compact;">Composer is a great way to manage your PHP dependencies. The documentation is still a bit scarce, so you could imagine that it took a while to find out how...</a>
</p>

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

<div class="sharedaddy sd-sharing-enabled">
  <div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing">
    <h3 class="sd-title">
      Share this:
    </h3>
    
    <div class="sd-content">
      <ul>
        <li class="share-twitter">
          <a rel="nofollow" data-shared="sharing-twitter-292" class="share-twitter sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=twitter" target="_blank" title="Click to share on Twitter"><span>Twitter</span></a>
        </li>
        <li class="share-google-plus-1">
          <a rel="nofollow" data-shared="sharing-google-292" class="share-google-plus-1 sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=google-plus-1" target="_blank" title="Click to share on Google+"><span>Google</span></a>
        </li>
        <li class="share-facebook">
          <a rel="nofollow" data-shared="sharing-facebook-292" class="share-facebook sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=facebook" target="_blank" title="Share on Facebook"><span>Facebook</span></a>
        </li>
        <li>
          <a href="#" class="sharing-anchor sd-button share-more"><span>More</span></a>
        </li>
        <li class="share-end">
        </li>
      </ul>
      
      <div class="sharing-hidden">
        <div class="inner" style="display: none;">
          <ul>
            <li class="share-print">
              <a rel="nofollow" data-shared="" class="share-print sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/" target="_blank" title="Click to print"><span>Print</span></a>
            </li>
            <li class="share-linkedin">
              <a rel="nofollow" data-shared="sharing-linkedin-292" class="share-linkedin sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=linkedin" target="_blank" title="Click to share on LinkedIn"><span>LinkedIn</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-reddit">
              <a rel="nofollow" data-shared="" class="share-reddit sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=reddit" target="_blank" title="Click to share on Reddit"><span>Reddit</span></a>
            </li>
            <li class="share-email">
              <a rel="nofollow" data-shared="" class="share-email sd-button share-icon" href="http://burgiblog.com/2012/03/08/depending-on-packages-without-composer-json-in-composer-php-dependency-manager/?share=email" target="_blank" title="Click to email this to a friend"><span>Email</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-end">
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</div>