---
id: 308
title: Google Analytics and RequireJS
author: Burgi
layout: post
guid: http://burgiblog.com/?p=308
permalink: /2013/07/09/google-analytics-and-requirejs/
categories:
  - Development
  - JavaScript
tags:
  - AMD
  - Google Analytics
  - how to
  - howto
  - JavaScript
  - RequireJS
---
<p class="wp-flattr-button">
  <a class="FlattrButton" style="display:none;" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/" title=" Google Analytics and RequireJS" rev="flattr;uid:BurkhardR;language:en_GB;category:audio;tags:AMD,Google Analytics,how to,howto,JavaScript,RequireJS,blog;button:compact;">If, like me, you use AMD-style loading for your JavaScript projects, you might have wondered how to integrate the (non-AMD enabled) Google Analytics library into your project. &nbsp; Define fallbacks...</a>
</p>

If, like me, you use AMD-style loading for your JavaScript projects, you might have wondered how to integrate the (non-AMD enabled) Google Analytics library into your project.

&nbsp;

## Define fallbacks

Many web apps these days are being built with offline capabilities in mind. Go ahead and download&nbsp;<http://www.google-analytics.com/analytics.js>&nbsp;to your local web folder. We&#8217;ll need it as a fallback in a second.

Add the following to your paths config:

<pre class="brush: javascript; gutter: true">paths: {
    ...
    "google-analytics":         [
        "//www.google-analytics.com/analytics",
        "vendor/google-analytics" // this is your local copy
    ]
}</pre>

## Shim it!

The GA library doesn&#8217;t contain any AMD-detection, so we&#8217;ll need to shim it and export the *ga* global.

<pre class="brush: javascript; gutter: true">shim: {
    ....
    "google-analytics":  {
        exports: "ga"
    }
}</pre>

## Usage

You&#8217;ll need to initialise it. Here&#8217;s a basic analytics module I&#8217;ve written that serves as an example:

&nbsp;

<pre class="brush: javascript; gutter: true">define(["eventbus", "google-analytics"], function (Eventbus, ga) {

    var self = {};

    self.trackPageView = function (uri) {
        // @todo check active ga/connection before each event
        ga(&#039;send&#039;, &#039;event&#039;, &#039;pageView&#039;, uri);
    };

    self.trackAction = function (type, description) {
        ga(&#039;send&#039;, &#039;event&#039;, type, description);
    };

    self.trackTiming = function (category, identifier, time) {
        time = time || new Date().getTime() - window.performance.timing.domComplete;
        ga(&#039;send&#039;, &#039;timing&#039;, category, identifier, time);
    };

    Eventbus.on("loading.finished", function () {
        ga(&#039;create&#039;, &#039;UA-XXXXXXX-X&#039;, {
            &#039;cookieDomain&#039;: &#039;none&#039;
        });
        ga(&#039;send&#039;, &#039;pageview&#039;);
        self.trackTiming("webapp", "initialise")
    });

    return self;

});</pre>

You&#8217;ll probably want to pass in the tracking code as a config parameter, and check for a valid ga session before each tracked event, or perhaps collect some events when offline and only fire them once back online. Go ahead and be creative!

<div class="sharedaddy sd-sharing-enabled">
  <div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing">
    <h3 class="sd-title">
      Share this:
    </h3>
    
    <div class="sd-content">
      <ul>
        <li class="share-twitter">
          <a rel="nofollow" data-shared="sharing-twitter-308" class="share-twitter sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=twitter" target="_blank" title="Click to share on Twitter"><span>Twitter</span></a>
        </li>
        <li class="share-google-plus-1">
          <a rel="nofollow" data-shared="sharing-google-308" class="share-google-plus-1 sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=google-plus-1" target="_blank" title="Click to share on Google+"><span>Google</span></a>
        </li>
        <li class="share-facebook">
          <a rel="nofollow" data-shared="sharing-facebook-308" class="share-facebook sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=facebook" target="_blank" title="Share on Facebook"><span>Facebook</span></a>
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
              <a rel="nofollow" data-shared="" class="share-print sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/" target="_blank" title="Click to print"><span>Print</span></a>
            </li>
            <li class="share-linkedin">
              <a rel="nofollow" data-shared="sharing-linkedin-308" class="share-linkedin sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=linkedin" target="_blank" title="Click to share on LinkedIn"><span>LinkedIn</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-reddit">
              <a rel="nofollow" data-shared="" class="share-reddit sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=reddit" target="_blank" title="Click to share on Reddit"><span>Reddit</span></a>
            </li>
            <li class="share-email">
              <a rel="nofollow" data-shared="" class="share-email sd-button share-icon" href="http://burgiblog.com/2013/07/09/google-analytics-and-requirejs/?share=email" target="_blank" title="Click to email this to a friend"><span>Email</span></a>
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