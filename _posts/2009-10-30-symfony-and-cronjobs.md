---
id: 199
title: symfony and Cronjobs
author: Burgi
excerpt: This short tutorial explains how to set up reoccuring tasks with symfony through cronjobs.
layout: post
guid: http://burgiblog.com/?p=199
permalink: /2009/10/30/symfony-and-cronjobs/
categories:
  - symfony
tags:
  - 11.1
  - cron
  - cronjob
  - crontab
  - how to
  - howto
  - job
  - symfony
  - tab
  - task
  - tutorial
---


In order to automate tasks that should run on a regular basis, you will need a cronjob. Fortunately, creating cronjobs with symfony is a piece of cake.

First, you will need to create a batch task. Navigate to your project directory and enter

    symfony generate:task [your task] 

This will generate a task skeleton for you in *lib/task/[your task]Task.class.php* , which contains two important methods:* configure()* and *execute()*. The names are pretty self-explanatory. All your code should go in the *execute()* method. You might want to do some configurations first. Change the following lines in *configure()* accordingly:

<pre>$this-&gt;namespace        = 'project';
$this-&gt;name             = '[name-for-your-task]';
$this-&gt;briefDescription = '[some short explanation of what your task does]';</pre>

Before you start hacking your code into the *execute()* method, I would recommend to test-run it in a normal symfony module first. When you&#8217;re done, copy&paste the action of your test module into the *execute()* method. Be aware that tasks don&#8217;t contain a view and, therefore, you will need to *echo *any output directly in the method itself. An example that, frankly speaking, does nothing useful at all:

<pre>protected function execute($arguments = array(), $options = array())
{
 // initialize the database connection
 $databaseManager = new sfDatabaseManager($this-&gt;configuration);
 $connection = $databaseManager-&gt;getDatabase($options['connection'] ? $options['connection'] : null)-&gt;getConnection();

 // add your code here

 echo "I just did nothing at all, but at least I did it successfully!\n\n";
}</pre>

Let&#8217;s give it a test run. Open your command line and enter

<pre>symfony project:[your task]</pre>

If you see the expected output, you&#8217;re ready to go to the next step.

**Please notice:** The paths I&#8217;ve used apply to OpenSuse 11.1. If you run another Linux distribution, you might have to amend them.

Open your servers crontab at */etc/crontab* in your favourite text editor and add the following line to the end of the document:

<pre>*/5 * * * * cd [YOUR SF APP DIR] && /usr/bin/symfony project:[YOUR TASK] &gt;&gt;[YOUR SF APP DIR]/log/crontab.log</pre>

***Update**: Thanks to Marcell Fülöp for the suggestion to use >> to avoid overriding existing log entries*

Explaining every detail of cronjobs would definitely blow the extent of this how-to and there are some very good <a href="http://www.thesitewizard.com/general/set-cron-job.shtml" target="_blank">sites</a> out there that can help you understand. Just a quick explanation: This task runs every 5 minutes, changes to your symfony application directory, executes your task (please note that you don&#8217;t have access to your $PATH variables, so no *symfony* shortcut!) and outputs the result to */log/crontab.log* .

