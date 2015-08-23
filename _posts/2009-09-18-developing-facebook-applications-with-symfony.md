---
id: 171
title: Developing Facebook and Facebook Connect applications with symfony
author: Burgi
excerpt: This step-by-step tutorial explains how to effectively use symfony for producing facebook applications. It mainly concentrates on configuration and also contains a real life example.
layout: post
guid: http://burgiblog.com/?p=171
permalink: /2009/09/18/developing-facebook-applications-with-symfony/
suf_pseudo_template:
  - default
categories:
  - Development
  - symfony
tags:
  - facebook
  - facebook connect
  - how to
  - howto
  - PHP
  - sfFacebookConnectPlugin
  - sfGuardPlugin
  - symfony
  - tutorial
---
<p class="wp-flattr-button">
  <a class="FlattrButton" style="display:none;" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/" title=" Developing Facebook and Facebook Connect applications with symfony" rev="flattr;uid:BurkhardR;language:en_GB;category:audio;tags:facebook,facebook connect,how to,howto,PHP,sfFacebookConnectPlugin,sfGuardPlugin,symfony,tutorial,blog;button:compact;">Call to all developers: Since this plugin is no longer being developed and is still using the old REST-interface, I&#8217;m looking into forking or developing a new Facebook plugin that...</a>
</p>

## *Call to all developers:*

<del><em>Since this plugin is no longer being developed and is still using the old REST-interface, I&#8217;m looking into forking or developing a new Facebook plugin that uses the new OAuth protocol. Please <a href="http://www.facebook.com/burgizinho" target="_blank">contact me</a> if you&#8217;re interested.</em></del>

*<a title="Web Agency in Leeds" href="http://www.madebypi.co.uk" target="_blank">We</a>&#8216;re currently working on an extensive symfony (1.4) plugin that builds upon the new Graph API. Watch this blog for further updates!  
*

* * *

We have just finished developing our first facebook application using symfony and the very useful <a href="http://www.symfony-project.org/plugins/sfFacebookConnectPlugin" target="_blank">sfFacebookConnectPlugin</a> which can be downloaded from the SVN repository (or from <a href="http://plugins.symfony-project.org/get/sfFacebookConnectPlugin/sfFacebookConnectPlugin-1.0.0.tgz" target="_blank">here</a> for those of you who don&#8217;t use SVN). Simply unpack it to the *plugins/* folder of your symfony project.

&nbsp;

But before you can use the sfFacebookConnectPlugin, you need to install sfGuard:

### Installing and configuring sfGuardPlugin and sfFacebookConnectPlugin

Open the command line shell (cmd on Windows, PuTTy/bash on remote systems/linux), change to your project directory and type:

<pre>symfony plugin-install http://plugins.symfony-project.com/sfGuardPlugin</pre>

to install the plugin from the official symfony repository.

Now open the *settings.yml* in */apps/[myapp]/config* and edit

<pre>all:
  .actions:
    login_module:           sfGuardAuth   # To be called when a non-authenticated user
    login_action:           signin     # Tries to access a secure page
    secure_module:          sfGuardAuth   # To be called when a user doesn't have
    secure_action:          secure    # The credentials required for an action
  .settings:
    enabled_modules:      [default, sfFacebookConnectAuth, sfGuardAuth]</pre>

Now open *myUser.class.php* in */[myapp]/lib* and update the myUser class to extend sfFacebookUser:

<pre>class myUser extends sfFacebookUser
{
}</pre>

Add the following lines to the end of your *routing.yml* at /[myapp]/config to enable the signin and signout (optional):

<pre>sf_guard_signin:
 url:   /login
 param: { module: sfGuardAuth, action: signin }

sf_guard_signout:
 url:   /logout
 param: { module: sfGuardAuth, action: signout }

sf_guard_password:
 url:   /request_password
 param: { module: sfGuardAuth, action: password }</pre>

Now edit the *[myapp]/config/app.yml* with all the settings you have obtained from <a href="http://www.facebook.com/developers/apps.php" target="_blank">http://www.facebook.com/developers/apps.php</a>:

<pre># default values
all:
 facebook:
   api_key: ...
   api_secret: ...
   api_id: ...
   callback_url: [your callback url]
   app_url: http://apps.facebook.com/[your app]/
   redirect_after_connect: true
   redirect_after_connect_url: 'http://apps.facebook.com/[your app]/'
   connect_signin_url: '[url to your app]/sfFacebookConnectAuth/signin'

 sf_guard_plugin:
   profile_class: sfGuardUserProfile
   profile_field_name: user_id
   profile_facebook_uid_name: facebook_uid
   profile_email_name: email
   profile_email_hash_name: email_hash

 facebook_connect:
   load_routing:     true
   user_permissions: []</pre>

Next, you want to modify your sfGuardUserProfile schema (*apps/[myapp]/config/schema.yml*) to contain these 3 additional rows needed. Simply add them to the end of *schema.yml*:

<pre>sf_guard_user_profile:
 _attributes:      { phpName: sfGuardUserProfile }
 id:
 user_id:     { type: integer, foreignTable: sf_guard_user, foreignReference: id, required: true, onDelete: cascade }
 nickname:         { type: varchar(32), index: unique }
 first_name:       varchar(20)
 last_name:        varchar(20)
 birthday:         date
 facebook_uid:     varchar(20)
 email:            varchar(255)
 email_hash:       varchar(255)</pre>

*Site note: Since the new Facebook UIDs are way bigger than integer could possibly store, we have decided to go with varchar here. The official README is outdated.*

*  
*

Clear your cache:

<pre>symfony cc</pre>

Publish your assets:

<pre>symfony plugin:publish-assets</pre>

And finally, rebuild your model and insert the generated SQL using the database settings you have provided in your */config/databases.yml*:

<pre>symfony propel:build-model
symfony propel:build-sql
symfony propel:build-forms
symfony propel:build-filters
symfony propel:insert-sql</pre>

### Using sfFacebookConnectPlugin* *

We called our module *facebook *and the layout *fb*. Of course these are just examples and you may adjust them to your own needs.

Here is our layout* fb.php*:

<pre>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en" xmlns:fb="http://www.facebook.com/2008/fbml"&gt;
&lt;head&gt;
&lt;?php use_helper('sfFacebookConnect') ?&gt;
&lt;?php include_http_metas() ?&gt;
&lt;?php include_metas() ?&gt;
&lt;?php include_title() ?&gt;
&lt;link rel="shortcut icon" href="/favicon.ico" /&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;?php echo $sf_content ?&gt;

&lt;?php include_bottom_facebook_connect_script(); ?&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>

Next, a very basic *actions.class.php*:

<pre>&lt;?php

/**
 * facebook actions.
 *
 * @package
 * @subpackage facebook
 * @author     Your name here
 * @version    SVN: $Id: actions.class.php 12479 2008-10-31 10:54:40Z fabien $
 */
class facebookActions extends sfActions
{
	/**
	 * Executes index action
	 *
	 * @param sfRequest $request A request object
	 */

	public function executeIndex(sfWebRequest $request)
	{
		sfFacebook::requireLogin();
		//get the user object
		$user = $this-&gt;getUser();

		// facebook user id
		$this-&gt;fb_uid = $user-&gt;getCurrentFacebookUid();
		// get or create user based on fb_uid
		$fb_user = sfFacebook::getOrCreateUserByFacebookUid($this-&gt;fb_uid);
	}</pre>

And finally, and example how to render a friends invite form in your *indexSuccess.php*:

<pre>&lt;?php if ($sf_user-&gt;isFacebookConnected()): ?&gt;

&lt;fb:serverfbml style="width: 740px;"&gt;
	&lt;script type="text/fbml"&gt;
        &lt;fb:fbml&gt;
        	&lt;fb:request-form target="_top" action="[where to redirect after invite]" method="post" type="[name of your app]" content="[text the user will receive]&lt;fb:req-choice url=&quot;http://apps.facebook.com/[your app]/&quot; label=&quot;Accept!&quot;  " image="" invite="true"&gt;
        		&lt;fb:multi-friend-selector cols="4" actiontext="[some text above the invite form]" /&gt;
	        &lt;/fb:request-form&gt;
        &lt;/fb:fbml&gt;
    &lt;/script&gt;
&lt;/fb:serverfbml&gt;

&lt;?php else: ?&gt;
&lt;p&gt;Ooops?&lt;/p&gt;
&lt;br /&gt;
&lt;?php endif; ?&gt;</pre>

I hope you enjoyed this tutorial!

P.S.: Thanks to Andrés for the hint!

P.P.S.: As outlined by Ben below:

> *The other thing which was an issue I found very hard to spot (I am still new to Symfony) is in the security.yml file in “modules/sfFacebookConnectAuth/config”, if you are using symfony 1.4 (I am not sure about other versions as I haven’t used them), you have to use “false” instead of “off” – if you find that the sfGuardUser record is being created, but the user is not being signed in, this is likely to be the problem. (I believe that this is a change in the syntax of YAML in newer versions). *

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 1681px; width: 1px; height: 1px;">
  <p>
    <?php
  </p>
  
  <p>
    /**<br /> * facebook actions.<br /> *<br /> * @package quiz4fun<br /> * @subpackage facebook<br /> * @author Your name here<br /> * @version SVN: $Id: actions.class.php 12479 2008-10-31 10:54:40Z fabien $<br /> */<br /> class facebookActions extends sfActions<br /> {<br /> /**<br /> * Executes index action<br /> *<br /> * @param sfRequest $request A request object<br /> */
  </p>
  
  <p>
    public function executeIndex(sfWebRequest $request)<br /> {<br /> sfFacebook::requireLogin();<br /> }
  </p>
  
  <p>
    public function executeApplet(sfWebRequest $request)<br /> {<br /> sfFacebook::requireLogin();<br /> //get the user object<br /> $user = $this->getUser();
  </p>
  
  <p>
    // facebook user id<br /> $this->fb_uid = $user->getCurrentFacebookUid();<br /> // get user based on fb_uid<br /> $fb_user = sfFacebook::getOrCreateUserByFacebookUid($this->fb_uid);
  </p>
  
  <p>
    // create a new session ID for authentication<br /> $this->tmp_session = $this->tmpSession();<br /> // write to db<br /> $fb_user->getProfile()->setTmpSession($this->tmp_session);<br /> $fb_user->getProfile()->save();
  </p>
  
  <p>
    // nickname<br /> $this->nickname = $fb_user->getProfile()->getNickname();<br /> if(!$this->nickname OR trim($this->nickname) == &#8221;)<br /> {<br /> // set nickname<br /> $this->redirect(&#8216;facebook/updateNickname?nick=fb_&#8217;.$this->fb_uid);<br /> }<br /> }
  </p>
</div>

<div class="sharedaddy sd-sharing-enabled">
  <div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing">
    <h3 class="sd-title">
      Share this:
    </h3>
    
    <div class="sd-content">
      <ul>
        <li class="share-twitter">
          <a rel="nofollow" data-shared="sharing-twitter-171" class="share-twitter sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=twitter" target="_blank" title="Click to share on Twitter"><span>Twitter</span></a>
        </li>
        <li class="share-google-plus-1">
          <a rel="nofollow" data-shared="sharing-google-171" class="share-google-plus-1 sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=google-plus-1" target="_blank" title="Click to share on Google+"><span>Google</span></a>
        </li>
        <li class="share-facebook">
          <a rel="nofollow" data-shared="sharing-facebook-171" class="share-facebook sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=facebook" target="_blank" title="Share on Facebook"><span>Facebook</span></a>
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
              <a rel="nofollow" data-shared="" class="share-print sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/" target="_blank" title="Click to print"><span>Print</span></a>
            </li>
            <li class="share-linkedin">
              <a rel="nofollow" data-shared="sharing-linkedin-171" class="share-linkedin sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=linkedin" target="_blank" title="Click to share on LinkedIn"><span>LinkedIn</span></a>
            </li>
            <li class="share-end">
            </li>
            <li class="share-reddit">
              <a rel="nofollow" data-shared="" class="share-reddit sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=reddit" target="_blank" title="Click to share on Reddit"><span>Reddit</span></a>
            </li>
            <li class="share-email">
              <a rel="nofollow" data-shared="" class="share-email sd-button share-icon" href="http://burgiblog.com/2009/09/18/developing-facebook-applications-with-symfony/?share=email" target="_blank" title="Click to email this to a friend"><span>Email</span></a>
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