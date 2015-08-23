---
id: 192
title: 'Eclipse IDE: Symfony essentials'
author: Burgi
excerpt: This tutorial explains how to make the Eclipse IDE ready for symfony (PHP framework).
layout: post
guid: http://burgiblog.com/?p=192
permalink: /2009/10/18/eclipse-ide-symfony-essentials/
categories:
  - symfony
tags:
  - Aptana
  - Eclipse
  - Essentials
  - how to
  - howto
  - PDT
  - PHP
  - symfony
  - tutorial
  - YAML
  - YEdit
---
<p class="wp-flattr-button">
  <a class="FlattrButton" style="display:none;" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/" title=" Eclipse IDE: Symfony essentials" rev="flattr;uid:BurkhardR;language:en_GB;category:audio;tags:Aptana,Eclipse,Essentials,how to,howto,PDT,PHP,symfony,tutorial,YAML,YEdit,blog;button:compact;">Every programmer knows it, every programmer loves it: the Eclipse IDE. It is probably the most extensive free open-source development environment out there (Netbeans put aside). In this tutorial I&#8217;d...</a>
</p>

<p style="text-align: center;">
  <a href="http://burgiblog.com/wp-content/uploads/2009/10/eclipse_sf.png"><img class="size-full wp-image-196 aligncenter" title="Eclipse and Symfony" src="http://burgiblog.com/wp-content/uploads/2009/10/eclipse_sf.png" alt="Eclipse and Symfony - A beautiful couple" width="482" height="106" /></a>
</p>

Every programmer knows it, every programmer loves it: the Eclipse IDE. It is probably the most extensive free open-source development environment out there (Netbeans put aside). In this tutorial I&#8217;d like to show you how you can get it ready for symfony, the (in my opinion) best PHP framework there is at the moment.

1. Download and install the Eclipse-based PDT (PHP Development Tools) standalone version: <a href="http://www.eclipse.org/pdt/" target="_blank">http://www.eclipse.org/pdt/</a>

2. Now we&#8217;re going to install some plugins that are going to make your life easier when you are a symfony developer. Click *Help -> Install New Software &#8230;* and add the following plugins to Eclipse:

  * Aptana Studio Update Site &#8211; http://update.aptana.com/update/studio/ -> Aptana Studio (only if you want to use the built-in FTP functionality, otherwise you don&#8217;t need to bother about this one)
  * YEdit &#8211; http://dadacoalition.org/yedit -> YEdit (a YAML syntax highlighter, since most of symfony&#8217;s configuration is being done through .yml files

3. After restarting Eclipse, we are going to add some symfony-specific auto-completion features to Eclipse. At this point, you will probably have the symfony framework installed somewhere on your PC. If you have not done so, download it from <a title="symfony 1.2 zip" href="http://www.symfony-project.org/get/symfony-1.2.9.zip" target="_blank">here</a> or <a title="symfony 1.2 tgz" href="http://www.symfony-project.org/get/symfony-1.2.9.tgz" target="_blank">here</a> and unpack it to a directory that you can easily remember.  
Now change back to Eclipse. Open *Window -> Preferences -> PHP -> PHP Libraries -> New*, enter &#8220;*symfony*&#8221; as a name and tick the *Add to environment* option. After clicking OK, you will notice the entry we just added in your libraries list. Mark the new entry by clicking on it, the click *Add external folder&#8230;*, browse to the folder where you put the symfony framework and add it.  
Now you can add the symfony library every time you create a new PHP Project in Eclipse or by right-clicking on an existing project *->Include Path -> Configure include path*. Either way, you need to click on *Add Library -> User Library -> symfony* and add it.

4. If you have installed the Aptana plugin before, you might want to activate the built-in shortcut for uploading the current file you are working on. Since that shortcut (*Ctrl + Shift + U*) is already reserved by Eclipse itself (and the Aptana team probably did not think of that when writing their framework), you need to do the following steps: *Window -> Preferences -> General -> Keys*, then do a search for &#8220;*Show Occurrences in File Quick Menu*&#8221; (filter) and unbind these shortcuts from this rather useless command (*Click -> Unbind Command*).  
You need to modify the Sync Manager (that is needed for uploading). Click *Window -> Show View -> Other -> Aptana Standard Views -> Sync Manager*. The configuration window will pop up at the bottom of Eclipse. Configuration is pretty much self-explanatory. Just make sure you have the FTP data for you symfony-enabled web server at hand.

**Now you can start developing beautiful symfony applications with Eclipse!** There are dozens of (video) tutorials about how to effectively use Eclipse out there, so you might want to have a look at these if you&#8217;ve never used Eclipse before (I absolutely recommend that, it will save you loads of time). To start a new project, select *File -> New -> PHP Project*, enter the necessary data and don&#8217;t forget to include the symfony library.

If you&#8217;re running you own server on localhost (a future tutorial will cover how to set one up with symfony enabled), another feature will come in quite handy as well: right-clicking on the project in your *PHP Explorer* and selecting *Command Line Shell* will start a shell with the project&#8217;s root folder. From there, you can start to use your symfony commands as usual.

Have fun developing symfony applications even quicker and easier!

<div class="sharedaddy sd-sharing-enabled">
  <div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing">
    <h3 class="sd-title">
      Share this:
    </h3>
    
    <div class="sd-content">
      <ul>
        <li class="share-twitter">
          <a rel="nofollow" data-shared="sharing-twitter-192" class="share-twitter sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=twitter" target="_blank" title="Click to share on Twitter"><span>Twitter</span></a>
        </li>
        <li class="share-google-plus-1">
          <a rel="nofollow" data-shared="sharing-google-192" class="share-google-plus-1 sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=google-plus-1" target="_blank" title="Click to share on Google+"><span>Google</span></a>
        </li>
        <li class="share-facebook">
          <a rel="nofollow" data-shared="sharing-facebook-192" class="share-facebook sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=facebook" target="_blank" title="Share on Facebook"><span>Facebook</span></a>
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
              <a rel="nofollow" data-shared="" class="share-print sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/" target="_blank" title="Click to print"><span>Print</span></a>
            </li>
            <li class="share-linkedin">
              <a rel="nofollow" data-shared="sharing-linkedin-192" class="share-linkedin sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=linkedin" target="_blank" title="Click to share on LinkedIn"><span>LinkedIn</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-reddit">
              <a rel="nofollow" data-shared="" class="share-reddit sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=reddit" target="_blank" title="Click to share on Reddit"><span>Reddit</span></a>
            </li>
            <li class="share-email">
              <a rel="nofollow" data-shared="" class="share-email sd-button share-icon" href="http://burgiblog.com/2009/10/18/eclipse-ide-symfony-essentials/?share=email" target="_blank" title="Click to email this to a friend"><span>Email</span></a>
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