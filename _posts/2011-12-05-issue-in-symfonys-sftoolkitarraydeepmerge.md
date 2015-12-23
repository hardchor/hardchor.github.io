---
id: 278
title: 'Issue in symfony&#8217;s sfToolkit::arrayDeepMerge()'
author: Burgi
layout: post
guid: http://burgiblog.com/?p=278
permalink: /2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/
categories:
  - Development
  - symfony
tags:
  - bug
  - config
  - symfony 1.4
  - YAML
  - yml
---

<pre>
all:
  javascripts:
    101: /js/one.js
    102: /js/two.js
    103:
      /js/three.js: { position: first }
</pre>
<p>`

&nbsp;

&nbsp;

Hope this helps if you should run into the same issue.

