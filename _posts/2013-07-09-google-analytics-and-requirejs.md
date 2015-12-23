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

