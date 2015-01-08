---
layout: post
title:  "saprfc and RadRails "
date:   2006-09-22 12:00:00
categories: general
---


<p>
<h3>Ruby On Rails and SAP</h3>
Last August, I wrote a weblog about <a href="http://www.piersharding.com/blog/archives/2005/08/sap_on_rails_1.html" target="_blank">SAP on Rails</a>.  In a follow up to that I'd like to explain how to import the example into an <a href="http://www.eclipse.org/" target="_blank">Eclipse</a> environment called <a href="http://www.radrails.org/" target="_blank">RadRails</a>.  RadRails is a specialised Eclipse install that contains a set of plugins for developing native <a href="">Ruby</a>, and <a href="http://www.rubyonrails.org" target="_blank">Ruby on Rails</a> applications.</p>
<p>
The point of this blog entry is to explain how to import that example into RadRails, thus enabling all the IDE lovers out there to explore the example in their favourite environment.  </p>
<p>
All kidding aside - RadRails is a nice way to break into Rails development as it aids in visualising how a Rails application holds together.
</p>
<p>
<h3>Getting the necessary packages</h3>
The instructions for installation vary according to your OS.  I will explain for both Linux, and win32.</p>
<p>
The basic breakdown is as follows:
<ul>
<li>Make sure that you have the RFCSDK</li>
<li>Install Ruby</li>
<li>Install RubyGems</li>
<li>Install Rails</li>
<li>Install RadRails</li>
<li>Import and configure the Exrates example</li>
<li>Install SAP::Rfc for Ruby</li> 
</ul>
</p>

<p>
<h3>Install the RFCSDK</h3>
Ensure that you have the RFCSDK installed on your platform  available from <a href="http://service.sap.com/connectors" target="_blank">http://service.sap.com/connectors</a>.  For Linux this must be in /usr/sap/rfcsdk - check that librfccm.so is available in /usr/sap/rfcsdk/lib, and that it can be found at run time (you may have to add /usr/sap/rfcsdk/lib into /etc/ld.so.conf, and run ldconfig to do it). </p>
<p>
For win32 (I've tested this on XP SP2) - make sure that the RFCSDK has been installed correctly, most likely as part of the SAP GUI client install, if you can't get it from <a href="http://service.sap.com/connectors" target="_blank">http://service.sap.com/connectors</a>.
</p>

<p>
<h3>Install Ruby, RubyGems, and Ruby on Rails</h3>

Detailed instructions for installing Ruby On Rails are found  on the <a href="http://www.rubyonrails.org/down" target="_blank"> rails </a> Website.  The following is the guide for the impatient:
</p>
<p>
<ul>
<li>Install Ruby from <a href="http://rubyforge.org/frs/download.php/9417/ruby184-16_rc1.exe">here</a> - it is *VERY* important to use this version for windows.  For Debian: apt-get install ruby1.8 ruby-1.8-dev irb ri (more details <a href="http://wiki.rubyonrails.org/rails/pages/HowtoInstallCompleteRubyOnDebian" target="_blank"> here </a>).  For other flavours of Linux, I'm afraid you'll have to figure that out for yourself, as I use <a href="http://www.ubuntulinux.org" target="_blank">Ubuntu</a> a Debian derivative.</li>
<li>Install <a href="http://docs.rubygems.org/" target="_blank">Ruby Gems</a> - get gems from <a href="http://rubyforge.org/frs/download.php/5208/rubygems-0.8.11.zip">here</a>, extract the archive, and then from the command line execute "ruby setup.rb" in the zip files  root directory.</li>
<li>Install Rails - from the command line execute "gem install rails --include-dependencies"</li>
<li>For windows users it's a good idea to reboot now, as not everything that is running will have picked up the new path etc. pointing to ruby</li>
</ul> 
</p>
<p>
<h3>Install RadRails</h3>

Make sure you have a working JRE - you can get one from <a href="http://www.sun.com/java" target="_blank">here</a> - I used version  5 update 06.</p>
<p>

Obtain and install RadRails - 0.6.2 can be downloaded from <a href="http://www.radrails.org/page/download" target="_blank">here</a>.  This is availble as a standalone IDE, and as a Plugin - what I show here is only dealing with the standalone version.
</p>
  
<p>
<h3>Importing the Exrates example</h3>
<ul>
<li>Download the Eclipse project directory for the ExRates example from <a href="http://www.piersharding.com/download/radrails_exrate.zip">here</a>.  Unpack the archive, somewhere handy.</li>
<li>launch RadRails, and when it asks for a workspace, point it at the rails sub directory that was unpacked above.</li>
<li>Once the application is launched we need to get some settings right:
<ul>
  <li>ruby installation - navigate to Window => Preferences => Ruby => Installed Interpreters.  Change the entry "mine" to point to your installed ruby.  For windows, this will most likely be C:\ruby\bin\ruby.exe.</li>
  <li>Again, in the preferences panel - set the locations of rdoc, and ri</li>
</ul>

</ul>
</p>
<p>
You should now have a workspace that looks like this:
<img src="http://www.piersharding.com/download/radrails_workspace1.png"/>
</p>

<p>
<h3>Installing SAP::Rfc for Ruby</h3>
This part is dependent on you successfully installing the RFCSDK above.</p>
<p>
If you are running win32 then download the saprfc 0.16 <a href="http://www.piersharding.com/download/ruby/saprfc-0.16-mswin32.gem">gem</a> and install from the command line with: "gem install saprfc-0.16-mswin32.gem".  Because this is a gem based install, the way that this is loaded into an application for use is different.  Because of this you will need to modify SAP4Rails.rb.  Navigate to this file in RadRails Exrates/lib/SAP4Rails.rb.
change line 3 from: require 'SAP/Rfc'
</p>
<p>
to:
<pre>
require 'rubygems'
require_gem 'saprfc'
</pre>
</p>
<p>
Under Linux, get the SAP::Rfc <a href="http://www.piersharding.com/download/ruby/saprfc-0.16.tar.gz">software</a>, unpack and follow the build instructions in the README file, which are basically:
<pre>
ruby extconf.rb
make
make install
</pre>
</ul>
</p>

<p>
<h3>Running the Exrates example</h3>
<ul>
<li>Reconfigure the SAP RFC connection information - navigate in the left hand file panel to the config/sap.yml file.  Modify the connection setting here, before launching the server, to point to your own R/3 system.</li>
<li>Run the ExRates WEBRick server (bottom Servers tab), and then check the console to see if the server started correctly (botton Console tab) - then point browser at http://localhost:3000</li>
</ul>
</p>

<p>
You should now see a running server:
</p>
<p>
<img src="http://www.piersharding.com/download/radrails_console1.png"/>
</p>

<p>
<h3>Trouble Shooting</h3>

If the WEBrick server does not appear to start correctly, then check the console, and the rails/Exrates/server.log, and development.log.  Most times the problems are going to be with the ruby, and rails install.</p>
<p>  As this example was developed under Linux, I have noticed that the ExratesServer defined in the bottom panel Servers tab of RadRails, needs to be recreated under windows.  To do this, highlight the entry, and delete (Red X for "Remove" at top of panel).  Then go File => New => Server => WEBRick Server.  This should give you a dialog box with Project, Name, and Port - accept the defaults, and try starting the server again.
</p>
<p>
If there is a problem with the RFC connection, then this will appear on the console log, and further debug information should be available in the rails/Exrates/dev_rfc file.
</p>

<p>
<h3>Wrapping it up</h3>
Hopefully, you now have everything up and running - Now all I need to do is to get Craig to add this one into the "Box" :-)
</p>
<p>
<h3>Credits:</h3>
<ul>
<li>Thanks go to Olivier Boudry, who tirelessly helped out with debugging win32 gem build issues.</li>
<li>Thanks also to Craig for picking up the ball and running with it</li>
</ul>
</p>

<div id="a000057more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September 22, 2006 11:06 AM</p>





