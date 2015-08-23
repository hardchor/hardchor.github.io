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
<p class="wp-flattr-button">
  <a class="FlattrButton" style="display:none;" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/" title=" Issue in symfony&#8217;s sfToolkit::arrayDeepMerge()" rev="flattr;uid:BurkhardR;language:en_GB;category:audio;tags:bug,config,symfony 1.4,YAML,yml,blog;button:compact;">I&#8217;ve had a bit of an odd issue where JavaScript files from a plugin I&#8217;d written weren&#8217;t being included for some reason. Turns out that there&#8217;s a bug / feature...</a>
</p>

I&#8217;ve had a bit of an odd issue where JavaScript files from a plugin I&#8217;d written weren&#8217;t being included for some reason.

Turns out that there&#8217;s a bug / feature (?) in the way symfony merges config files.<!--more-->

*plugins/myPlugin/config/view.yml*  
`</p>
<pre>
all:
  javascripts:
    - /js/one.js
    - /js/two.js
    - /js/three.js
</pre>
<p>`

*app/frontend/config/view.yml*  
`</p>
<pre>
all:
  javascripts:
    - /js/four.js
    - /js/five.js
</pre>
<p>`

&nbsp;

**Expected result:**

`</p>
<pre>
all:
  javascripts:
    - /js/one.js
    - /js/two.js
    - /js/three.js
    - /js/four.js
    - /js/five.js
</pre>
<p>`

However, due to the way *arrayDeepMerge* merges the two arrays, all values from the first array are being overridden, resulting in:

`</p>
<pre>
all:
  javascripts:
    - /js/four.js
    - /js/five.js
    - /js/three.js
</pre>
<p>`

&nbsp;

This will happen in all config files, most notably *view.yml, app.yml, settings.yml,* etc. (this has been discussed in http://groups.google.com/group/symfony-devs/browse_thread/thread/3a725483db3dec82 but lies vacant ever since 2007).

&nbsp;

A workaround would be to specify a unique index for your javascript, e.g.:

*plugins/myPlugin/config/view.yml*  
`</p>
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

<div class="sharedaddy sd-sharing-enabled">
  <div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing">
    <h3 class="sd-title">
      Share this:
    </h3>
    
    <div class="sd-content">
      <ul>
        <li class="share-twitter">
          <a rel="nofollow" data-shared="sharing-twitter-278" class="share-twitter sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=twitter" target="_blank" title="Click to share on Twitter"><span>Twitter</span></a>
        </li>
        <li class="share-google-plus-1">
          <a rel="nofollow" data-shared="sharing-google-278" class="share-google-plus-1 sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=google-plus-1" target="_blank" title="Click to share on Google+"><span>Google</span></a>
        </li>
        <li class="share-facebook">
          <a rel="nofollow" data-shared="sharing-facebook-278" class="share-facebook sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=facebook" target="_blank" title="Share on Facebook"><span>Facebook</span></a>
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
              <a rel="nofollow" data-shared="" class="share-print sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/" target="_blank" title="Click to print"><span>Print</span></a>
            </li>
            <li class="share-linkedin">
              <a rel="nofollow" data-shared="sharing-linkedin-278" class="share-linkedin sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=linkedin" target="_blank" title="Click to share on LinkedIn"><span>LinkedIn</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-reddit">
              <a rel="nofollow" data-shared="" class="share-reddit sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=reddit" target="_blank" title="Click to share on Reddit"><span>Reddit</span></a>
            </li>
            <li class="share-email">
              <a rel="nofollow" data-shared="" class="share-email sd-button share-icon" href="http://burgiblog.com/2011/12/05/issue-in-symfonys-sftoolkitarraydeepmerge/?share=email" target="_blank" title="Click to email this to a friend"><span>Email</span></a>
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