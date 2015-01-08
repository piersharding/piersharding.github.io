---
layout: post
title:  "saprfc Ruby on Rails with AJAX"
date:   2006-09-29 12:00:00
categories: general
---


Since I wrote an <a href="http://www.piersharding.com/blog/archives/2005/08/sap_on_rails_1.html" target="_blank">article</a> about integration of SAP and <a href="http://www.rubyonrails.com" target="_blank">Ruby on Rails</a>, it has been great to see the beginnings of a kernel of interest.  As a result I decided to package up the sap4rails code and distribute it properly on <a href="http://raa.ruby-lang.org/project/sap4rails/" target="_blank">RAA</a>.
<br>
<h3>Rails and AJAX</h3>
One of the interesting things that Ruby on Rails provides is built AJAX functionality by virtue of an <a href="http://api.rubyonrails.org/classes/ActionView/Helpers/JavaScriptHelper.html" target="_blank">API</a> over <a href="http://prototype.conio.net/" target="_blank">prototype</a>, and <a href="http://script.aculo.us/" target="_blank">Scriptalicious</a>.  In this blog, I would like to show how neatly this integration is implemented in RubyOnRails, using a simple example of Locking, and Unlocking SAP R/3 user accounts.
<br>
<h3>Installation Requirements</h3>
For this example you will need to do your own install of Ruby On Rails.  Use the instructions for installing Ruby, Ruby On Rails, the RFCSDK, and saprfc in the <a href="http://www.piersharding.com/blog/archives/2005/08/sap_on_rails_1.html" target="_blank">RadRails</a> blog.<br>
Just be sure to install versions Ruby 1.8.4+ and Rails 1.1.2+.
<br>
Once you have got this far, the last thing to install is <a href="http://raa.ruby-lang.org/project/sap4rails/" target="_blank">sap4rails</a>.  You can either install the <a href="http://www.piersharding.com/download/ruby/rails/">source package</a> or download the <a href="http://www.piersharding.com/download/ruby/rails/">gem</a>, and install this with:<br>
 <pre>gem install sap4rails-&lt;version&gt;.gem</pre>.  
<h3>UserAdmin</h3>
For this example, most of the standard BAPIs are adequate.  We need to be able to list users, with their details, including their lock state.  We also need to be able to lock and unlock them.
<br>
The general functionality of the application is to create two lists of users
on a page - locked and unlocked - and for you to be able to drag a user from
one to the other to change their lock state in SAP.
<br>
<p>
The following RFCs are used:
<ul>
<li>BAPI_USER_GETLIST - list users, and their address details </li>
<li>BAPI_USER_LOCK</li>
<li>BAPI_USER_UNLOCK</li>
</ul>
</p>
<p>
BAPI_USER_GETLIST is not quite enough.  This, I have had to wrap in another function module and also modify the results table to include the lock status information of users.
<br>
Create a new function module called Z_BAPI_USER_GETLIST, and make sure that
you activate it for RFC on the attributes tab (in SE37) <a href="http://www.piersharding.com/download/ruby/rails/z_bapi_user_getlist.txt" target="_blank">(code)</a>.
<br>
Create a new structure called ZBAPIUSNAME, and include the two structures BAPIUSNAME, and USLOCK like this <a href="http://www.piersharding.com/download/ruby/rails/zbapiusname.png" target="_blank"><img src="http://www.piersharding.com/download/ruby/rails/zbapiusname.thumb.png" /></a>.
<br>
  Make sure that you activate the structure.
</p>
<br>
<p>
The code and interface needs to be completed like this:
<pre>
FUNCTION Z_BAPI_USER_GETLIST.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(MAX_ROWS) TYPE  BAPIUSMISC-BAPIMAXROW DEFAULT 0
*"     VALUE(WITH_USERNAME) TYPE  BAPIUSMISC-WITH_NAME DEFAULT SPACE
*"  EXPORTING
*"     VALUE(ROWS) TYPE  BAPIUSMISC-BAPIROWS
*"  TABLES
*"      SELECTION_RANGE STRUCTURE  BAPIUSSRGE OPTIONAL
*"      SELECTION_EXP STRUCTURE  BAPIUSSEXP OPTIONAL
*"      USERLISTLOCK STRUCTURE  ZBAPIUSNAME OPTIONAL
*"      RETURN STRUCTURE  BAPIRET2 OPTIONAL
*"----------------------------------------------------------------------
*
data:
  LOCKSTATE LIKE  USLOCK,
  userlist like bapiusname occurs 0 with header line.

  refresh userlistlock.


  CALL FUNCTION 'BAPI_USER_GETLIST'
    EXPORTING
      MAX_ROWS              = 0
      WITH_USERNAME         = with_username
    IMPORTING
      ROWS                  = rows
    TABLES
      SELECTION_RANGE       = selection_range
      SELECTION_EXP         = selection_exp
      USERLIST              = userlist
      RETURN                = return
            .

  loop at userlist.
    move-corresponding: userlist to userlistlock.
    if userlistlock-firstname = space.
       userlistlock-firstname = userlistlock-username.
    endif.

    CALL FUNCTION 'SUSR_USER_LOCKSTATE_GET'
      EXPORTING
        USER_NAME                 = userlist-username
      IMPORTING
        LOCKSTATE                 = lockstate
      EXCEPTIONS
        USER_NAME_NOT_EXIST       = 1
        OTHERS                    = 2
              .

    move-corresponding: lockstate to userlistlock.
    append userlistlock.

  endloop.

ENDFUNCTION.
</pre>
</p>
<p>
Activate the function module and test it.
</p>

<h3>The Rails part</h3>
<p>
The full application can be downloaded from <a href="http://www.piersharding.com/download/ruby/rails/useradmin.tar.gz">here</a> - but what I'd
like to do is quickly describe the meat of what had to be done to get this
type of application working.
</p>
<h4>Config - sap.yml</h4>
<p>
As described in the <a href="https://weblogs.sdn.sap.com/pub/wlg/3516" target="_blank">RadRails</a> blog, you need to adjust the configuration in
config/sap.yml to point to your SAP system:
</p>
<p>
<pre>
development:
  ashost: 10.1.1.1
  sysnr: "00"
  client: "010"
  user: developer
  passwd: developer
  lang: EN
  trace: "1"
...
</pre>
</p>


<h4>Model - sap_user.rb</h4>
<p>
The SapUser object now inherits from the new SAP4Rails::Base class.  This
serves to automatically take care of managing RFC connections based on the
config done above.  The two main class methods for use are function_module,
which allows you to declare what RFCs you want to use, and parameter which is
a helper method for declaring attributes of a SapUser (or any other Model
object).
<br>
In the interests of simplifying the application, by reducing the amount of
ABAP code to be written, and the number of RFC calls to be made, I have in a
way "cheated", with the arrangement of methods defined in SapUser.  Instead of
having a SapUser#find method, I rely on the use of SapUser#find_all and
SapUser#find_cache to reduce a series of SapUser searches down to one RFC call
only.  In reality this is probably not good practice, but it suits for this
example.
<br>
<p>
Read the code comments below for further details:
</p>
<pre>
require_gem "sap4rails"

class SapUser < SAP4Rails::Base

# You must define a list of RFCs to preload
  function_module :Z_BAPI_USER_GETLIST,
	                :BAPI_USER_LOCK,
	                :BAPI_USER_UNLOCK

# You must define a list of attribute accessors to preload
  parameter :last, :first, :userid, :locked

# do your attribute initialisation for each SapUser instance
	def initialize(last, first, userid, locked)
	  @last = last
	  @first = first
	  @userid = userid
	  @locked = locked
	  @changed = false
	end

# what is the lock state
	def locked?
	  return self.locked ? true : false
	end

# on #save - flip the lock state of the SapUser, calling the 
# appropriate RFC to do it
  def save()
    RAILS_DEFAULT_LOGGER.warn("[SapUser]#save what did we get: " + self.inspect)
   
    if self.locked?
      SapUser.BAPI_USER_LOCK.reset()
      SapUser.BAPI_USER_LOCK.username.value = self.userid
      SapUser.BAPI_USER_LOCK.call()
    else
      SapUser.BAPI_USER_UNLOCK.reset()
      SapUser.BAPI_USER_UNLOCK.username.value = self.userid
      SapUser.BAPI_USER_UNLOCK.call()
    end

    # just so something happens ...
    return true
  end

# one RFC call to get them all
  def self.find_all
    RAILS_DEFAULT_LOGGER.warn("[SapUser]#find_all ")
    SapUser.Z_BAPI_USER_GETLIST.reset()
    SapUser.Z_BAPI_USER_GETLIST.with_username.value = 'X'
    SapUser.Z_BAPI_USER_GETLIST.call()
    users = []
    SapUser.Z_BAPI_USER_GETLIST.userlistlock.rows().each {|row|
		  next if row['FIRSTNAME'].strip.length == 0
		  state = nil
		  if row['WRNG_LOGON'] == "L" || 
                     row['LOCAL_LOCK'] == "L" || 
                     row['GLOB_LOCK'] == "L"
		    state = true
	          else
		    state = false
		  end
		  users.push(SapUser.new(row['LASTNAME'], 
			                 row['FIRSTNAME'], 
				         row['USERNAME'], 
					 state))
		}
    return users
  end

# find a user base on the results of a SapUser#find_all
  def self.find_cache(user, cache)
    RAILS_DEFAULT_LOGGER.warn("[SapUser]#find_cache: #{user} ")
    cache.each{|row|
	  return row if user.strip == row.userid.strip
    }
  end

# get a list of all the locked users
  def self.find_locked
    RAILS_DEFAULT_LOGGER.warn("[SapUser]#find_locked ")
    locked = []
    find_all().each{|user|
	  locked.push(user) if user.locked
    }
    return locked
  end

# get a list of all the unlocked users
  def self.find_unlocked
    RAILS_DEFAULT_LOGGER.warn("[SapUser]#find_unlocked ")
    unlocked = []
    find_all().each{|user|
	  unlocked.push(user) unless user.locked
    }
    return unlocked
  end
end
</pre>

<p>

<h4>Controller - lock_controller.rb</h4>
There are only 3 basic actions to the only controller in this application.
The initial list action, build the starting page presenting the two lists of
users (locked and unlocked).  From there, as a result of the AJAX enabled
calls from the dragndrop feature, two further actions are called - set_locked
and set_unlocked.
</p>
<pre>

class LockController < ApplicationController

# gnerate the starting user lists, and hand off to the default list view
  def list
    RAILS_DEFAULT_LOGGER.warn("[LIST] Parameters: " + @params.inspect)
    @locked_users = SapUser.find_locked()
    @unlocked_users = SapUser.find_unlocked()
    RAILS_DEFAULT_LOGGER.warn("[LIST] of Locked: " + @locked_users.inspect)
    RAILS_DEFAULT_LOGGER.warn("[LIST] of UNLocked: " + @unlocked_users.inspect)
  end

# check through the list of locked users in the locked sortable_element box
# and set their locked state in necessary
# on completion - render the partial locked_users
  def set_locked
    RAILS_DEFAULT_LOGGER.warn("[SET_LOCKED] Parameters: " + @params.inspect)
    @locked_users = []
    cache = SapUser.find_all()
    if @params['locked_box']
      @params['locked_box'].each {|locked|
	  next if locked.length == 0
          user = SapUser.find_cache(locked, cache)
	  next if user.first.strip.length == 0
	  if user && ! user.locked?
  	    user.locked = !user.locked?
   	    user.save
          end
          @locked_users.push(user)
      }
    end
    render :partial => 'locked_users', :object => locked_users
  end

# exact opposite/the same as set_locked
  def set_unlocked
    RAILS_DEFAULT_LOGGER.warn("[SET_UNLOCKED] Parameters: " + @params.inspect)
    @unlocked_users = []
    cache = SapUser.find_all()
    if @params['unlocked_box']
      @params['unlocked_box'].each {|unlocked|
	  next unless unlocked.length > 0
	  user = SapUser.find_cache(unlocked, cache)
	  next if user.first.strip.length == 0
	  if user && user.locked?
	    user.locked = !user.locked?
  	    user.save
  	  end
	  @unlocked_users.push(user)
      }
    end
    render :partial => 'unlocked_users', :object => unlocked_users
  end

# called by the rendering action of the partial locked_users
  def locked_users
    RAILS_DEFAULT_LOGGER.warn("[LOCKED_USERS] of Locked: " + @locked_users.inspect)
    @locked_users
  end

# called by the rendering action of the partial unlocked_users
  def unlocked_users
    RAILS_DEFAULT_LOGGER.warn("[UNLOCKED_USERS] of UNLocked: " + @unlocked_users.inspect)
    @unlocked_users
  end

end
</pre>
<p>
<h4>Views </h4>
The the overall page template (layout) defines the shape of the page, and what
JavaScript libraries are pulled in for the effects (AJAX).  All pages inherit from this.
</p>
<p>
<b>layout/application.rhtml</b>
</p>
<pre>
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
  &lt;head&gt;
    &lt;meta http-equiv="Content-type" content="text/html; charset=utf-8" /&gt;
    &lt;title&gt;User Administration:  &lt;%= controller.controller_name %&gt;&lt;/title&gt;
    &lt;meta http-equiv="imagetoolbar" content="no" /&gt;
    &lt;%= stylesheet_link_tag "administration.css" %&gt;
    &lt;%= javascript_include_tag "prototype", "effects", "dragdrop",
                       "controls" %&gt;
  &lt;/head&gt;

  &lt;body&gt;
  &lt;div id="container"&gt;
    &lt;div id="header"&gt;
    &lt;div id="info"&gt;
      &lt;%= link_to "home", :controller=&gt; "/lock", :action =&gt; 'list' %&gt;
    &lt;/div&gt;
      &lt;h1&gt;&lt;%= link_to "UserAdmin - #{controller.controller_name}", :controller =&gt; "/lock" %&gt;
        &lt;/h1&gt;
    &lt;/div&gt;
    &lt;div id="content"&gt;
      &lt;h2&gt;&lt;%= @page_heading %&gt;&lt;/h2&gt;
      &lt;%= @content_for_layout %&gt;
    &lt;/div&gt;
  &lt;/div&gt;
  &lt;/body&gt;
&lt;/html&gt;

</pre>

<p>
<b> lock/list.rhtml</b>
</p>
<p>
The two most important things in list are the two container div tags -
unlocked_box and locked_box.  These in turn, have a corresponding partial
(unlocked_users, and locked_users), that are responsible for generating the
dragable user items.
</p>
<pre>
&lt;% @heading = "User Admin - Lock/UnLock" %&gt;

  &lt;div id="user-admin"&gt;
    &lt;div id="unlocked" class="dropbox"&gt;
      &lt;h3&gt;Unlocked Users&lt;/h3&gt;
      &lt;div id="unlocked_box"&gt;
        &lt;%= render :partial =&gt; 'unlocked_users', :object =&gt; @unlocked_users %&gt;
      &lt;/div&gt;
    &lt;/div&gt;

    &lt;div id="cnt-locked" class="dropbox"&gt;
      &lt;h3&gt;Locked Users&lt;/h3&gt;
      &lt;div id="locked_box"&gt;
        &lt;%= render :partial =&gt; 'locked_users', :object =&gt; @locked_users %&gt;
      &lt;/div&gt;
    &lt;/div&gt;
    &lt;br clear="all" /&gt;

  &lt;/div&gt;

</pre>

<p><b> lock/_unlocked_users.rhtml</b></p>
<p>the partial unlocked_users either displays a place holder element if there
are no users, or calls the render of unlocked_user for each user.
It also uses the AJAX function sortable_element which dictates what div
container holds the sortable drag and drop elements, and what actions to take
when an event is fired with them.  This is how we trigger the call to the
set_unlocked or set_locked action of the list  controller for updating the
individual "boxes" of users.
</p>
<pre>

&lt;% if unlocked_users.empty? %&gt;
  &lt;div class="target"&gt;  You have no Unlocked SAP Users.... &lt;/div&gt;
&lt;% else %&gt;
  &lt;%= render :partial =&gt; 'unlocked_user', :collection =&gt; unlocked_users %&gt;
&lt;% end %&gt;

&lt;%= sortable_element "unlocked_box",
  :update =&gt; "unlocked_box",
  :url =&gt; {:action=&gt;'set_unlocked'},
  :tag =&gt; 'div', :handle =&gt; 'handle', :containment =&gt; ['unlocked_box','locked_box'] %&gt;

&lt;%= sortable_element "locked_box",
  :update =&gt; "locked_box",
  :url=&gt; {:action=&gt;'set_locked'},
  :tag =&gt; 'div', :handle =&gt; 'handle', :containment =&gt; ['locked_box','unlocked_box'] %&gt;

</pre>


<p><b>lock/_unlocked_user.rhtml</b></p>
<p>
the partial unlocked_user renders a dragble_element for each SapUser.
</p>
<pre>

&lt;div id="unlockeduser_&lt;%= unlocked_user.userid %&gt;" class="dragitem"&gt;
  &lt;h4 class="handle"&gt;&lt;%= unlocked_user.userid %&gt;&lt;/h4&gt;
  &lt;p&gt;&lt;%= unlocked_user.last + ", " + unlocked_user.first %&gt;&lt;/p&gt;
&lt;/div&gt;
&lt;%= draggable_element "unlockeduser_#{unlocked_user.userid}" %&gt;

</pre>


<p><b>lock/_locked_users.rhtml</b></p>
<p> the same as for the partial unlocked_users</p>
<pre>
&lt;% if locked_users.empty? %&gt;
  &lt;div class="target"&gt;  You have no Locked Users ...  &lt;/div&gt;
&lt;% else %&gt;
 &lt;%= render :partial =&gt; 'locked_user', :collection =&gt; locked_users %&gt;
&lt;% end %&gt;

&lt;%= sortable_element "unlocked_box",
  :update =&gt; "unlocked_box",
  :url =&gt; {:action=&gt;'set_unlocked'},
  :tag =&gt; 'div', :handle =&gt; 'handle', :containment =&gt; ['unlocked_box','locked_box'] %&gt;

&lt;%= sortable_element "locked_box",
  :update =&gt; "locked_box",
  :url=&gt; {:action=&gt;'set_locked'}, 
  :tag =&gt; 'div', :handle =&gt; 'handle', :containment =&gt; ['locked_box','unlocked_box'] %&gt;

</pre>


<p><b>lock/_locked_user.rhtml</b></p>
<p> the same as for the partial unlocked_user</p>
<pre>
&lt;div id="lockeduser_&lt;%= locked_user.userid %&gt;" class="dragitem"&gt;
  &lt;h4 class="handle"&gt;&lt;%= locked_user.userid %&gt;&lt;/h4&gt;
  &lt;p&gt;&lt;%= locked_user.last + ", " + locked_user.first %&gt;&lt;/p&gt;
&lt;/div&gt;
&lt;%= draggable_element "lockeduser_#{locked_user.userid}" %&gt;

</pre>

<p>
In config/routes.rb
add:
<pre>
 map.connect '', :controller => "lock", :action => 'list'
 </pre>

and make sure that you delete public/index.html
</P>
<p>
This makes sure that any requests to the root of the server eg.
http://localhost:3000, are forwarded onto the list action of the lock
controller.
</p>

<h4>firing Up</h4>
<p>
Start the Rails WEBrick server by running the script:
<pre>ruby scripts/server</pre>
Open your broswer and point to http://localhost:3000.
</p>
<p>
When you connect to http://localhost:3000 (there will be an inital delay as
sap4rails caches the RFC calls), you should get a screen that looks like this <br> <a href="http://www.piersharding.com/download/ruby/rails/useradmin.png" target="_blank"><img src="http://www.piersharding.com/download/ruby/rails/useradmin.thumb.png" /></a>
</p>
<p>
A Flash movie of this in action can be seen <a href="http://www.piersharding.com/download/ruby/rails/useradmin.html">here</a>.
</p>



<div id="a000058more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September 29, 2006 11:08 AM</p>





