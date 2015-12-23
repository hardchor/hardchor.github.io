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

