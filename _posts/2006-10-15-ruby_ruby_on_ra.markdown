---
layout: post
title:  "Ruby, Ruby on Rails, and SAP Web Services Integration"
date:   2006-10-15 12:00:00
categories: general
---


<p>
Something I like about Scripting Languages is the way they revel in having "more than one way to skin a cat".  So, in this spirit I have built a complementary interface to saprfc (for Ruby) called sapwas, that facilitates RFC calls via SAP Web Services.  This has been integrated into sap4rails, and the attached example demonstrates how to substitue Web Services for RFC integration in Ruby on Rails.
</p>
<h3>SAP Web Services vs RFC?</h3>
<p>
Other than curiosity, there is another motivation for trying this out - Since there has been discussion of late on the merits of RFC and Web Service technology such as SOAP, I thought that inorder to do the subject justice I would do some further (tangible) investigation.
</p>
<p>
To me, the most obvious way to draw a comparison, is to develop comparable examples of each, with the only difference being the substitution of the technology in question.
</p>
<p>This led to me focusing on a recent article I wrote: 
<a href="http://www.piersharding.com/blog/archives/2006/09/saprfc_ruby_on.html" target="_blank">Ruby on Rails with AJAX</a>, to use as a base line.</p>

<h3>Activating the SAP Web Service support</h3>
<p>
If we take the example above (<a href="http://www.piersharding.com/blog/archives/2006/09/saprfc_ruby_on.html" target="_blank">Ruby on Rails with AJAX</a>), then the changes are as follows:
<ul>
<li>Install sapwas (version 0.02+)</li>
<li>Upgrade sap4rails (version 0.04+)</li>
<li>modify the SapUser model</li>
<li>set your Rails configuration</li>
</ul>
</p>
<h3>Install sapwas</h3>
<p>
Download either the source distribution, or the gem file  by following the <a href="http://raa.ruby-lang.org/project/sapwas/" target="_blank">sapwas project</a> download links.  Gem files are easier to deal with - all you need to do is:
</p>
<p>
<pre>
gem install sapwas-&lt;version&gt;.gem
</pre>
</p>
<p>
And its done (use gem uninstall to remove if its there allready).
</p>
<p> You will probably need to install <a href="http://raa.ruby-lang.org/project/http-access2/" target="_blank">http-access2</a> inorder to get the  basic authentication working correctly.  This is a requirement of the <a href="http://raa.ruby-lang.org/project/soap4r/" target="_blank">SOAP4R</a> library.
</p>
<h3>Upgrade sap4rails</h3>
<p>
Download either the source distribution, or the gem file   for sap4rails by following the <a href="http://raa.ruby-lang.org/project/sap4rails/" target="_blank">project</a> download links.  First remove the old version, and install the new:
</p>
<p>
<pre>
gem uninstall sap4rails
gem install sap4rails-&lt;version&gt;.gem
</pre>
</p>
<p>
If you have used the source distribution previously, then you will have to manually remove the previous one, and install again (the usual ruby setup.rb dance - gems are soo much easier :-).
</p>
<h3>Modify the SapUser model</h3>
<p>
In the Rails application, edit the app/models/sap_user.rb, and change the super class for SapUser:
<pre>
class SapUser < SAP4Rails::WS::Base
...
</pre>
</p>
<h3>Set your Rails configuration</h3>
<p>
Modify the config/sap.yml file - you really should only need to add in the :url line depicted below, but check anyway:
</p>
<p>
<pre>  client: "010"
  url: "http://seahorse.local.net:8000/sap/bc/soap/rfc"
  user: developer
  passwd: developer
  lang: EN
</pre>
</p>
<h4>note on WAS configuration</h4>
<p>
You must ensure that /sap/bc/soap/rfc is configured/activated correctly in transaction SICF.
</p>
<h3>Go!</h3>
<p>
Once you have completed these changes, you can then test the Rails application as described in <a href="http://www.piersharding.com/blog/archives/2006/09/saprfc_ruby_on.html" target="_blank">Ruby on Rails with AJAX</a> - it should function in exactly the same manner - wasn't that easy!
</p>
<p>
Now you have it working - I'll leave you to draw your own conclussions on it's merits :-)
</p>

<div id="a000060more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at October 15, 2006  6:11 PM</p>





