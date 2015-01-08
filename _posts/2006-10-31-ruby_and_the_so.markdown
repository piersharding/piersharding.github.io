---
layout: post
title:  "Ruby and the SOAP RunTime (SRT) Handler"
date:   2006-10-31 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2006/10/sap4rails_007_s.html">&laquo; sap4rails 0.07 - SAP RFC and auto-reconnect</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/11/repensando_a_we.html">Repensando a web com Rails &raquo;</a>

</p>

<h2>October 31, 2006</h2>

<h3>Ruby and the SOAP RunTime (SRT) Handler</h3>

<p>
Following on from my previous post about <a href="http://www.piersharding.com/blog/archives/2006/10/ruby_ruby_on_ra.html" target="_blank">Ruby, Ruby on Rails, and SAP Web Services Integration</a> - I would like to show how to switch to using the SOAP RunTime (SRT) Handler, which makes available SAP Web Services via Virtual Interfaces.
</p>
<p>
<h3>Steps</h3>
<ul>
<li>Follow the steps outlined in <a href="http://www.piersharding.com/blog/archives/2006/09/saprfc_ruby_on.html" target="_blank">Ruby, Ruby on Rails, and SAP Web Services Integration</a> upgrading sapwas to at least version 0.06, and sap4rails to at least version 0.05, making sure that the soap/rfc handler works correctly.</li>
<li>Create the Virtual Interface, Web Service Definition, and Web Service Config to expose the UserAdmin Function Modules</li>
<li>Modify app/models/sap_user.rb to point to the new Web Service definition</li>
</ul>
</p>
<p>
<h3>Create the SAP Web Service</h3>
We need to create a web service that exposes the function modules that were used in the UserAdmin Rails application.  To do this - go to transaction SE80, go to the Enterprise Services tab, and create a Virtual Interface of type Funciton Group called Z_USERADMIN.  Include into this the function modules:
</p>
<p>
<ul>
<li>Z_BAPI_USER_GETLIST</li>
<li>BAPI_USER_GET_DETAIL</li>
<li>BAPI_USER_LOCK</li>
<li>BAPI_USER_UNLOCK</li>
</ul>
</p>
<p>
Activate this, and then create a Web Services definiton - again, using SE80, go to the Enterprise Services tab, and create a Web Services definition called Z_USERADMIN - referencing the previously activated Vitual Interface Z_USERADMIN.<br>
Finally - activate this in the ICF configuration (SICF), by using transaction WSCONFIG, referencing the Web Service definition Z_USERADMIN created above.
<br>
There is an excellent discussion of the details of this process by <a href="https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/u/251694270">Thomas Jung</a>, <a href="https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/wlg/3329" target="_blank">here</a>, and <a href="https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/wlg/1135" target="_blank">here</a>.
</p>
<p>
<h3>Modify model sap_user.rb</h3>
Now for the final part - in the UserAdmin Rails application, edit the SapUser model file app/models/sap_user.rb.  This needs to switch from referencing the method "function_module" for loading the functions, to using "resources", as outlined below:
<pre>
require_gem "sap4rails"

class SapUser < SAP4Rails::WS::Base

# You must define a list of RESOURCES to preload
   resources "http://seahorse.local.net:8000/sap/bc/srt/rfc/sap/Z_USERADMIN"
...
</pre>
Now you can test it as in the previous weblogs.
</p>
<p>
<h3>Unicode!</h3>
One thing that I neglected to say in my previous post, is a  major advantage of using sapwas for accessing SAP is that it has comprehensive Unicode support - free of charge (but not of pain) :-) .
</p>
<p>
<h3>Round Up</h3>
For me - this rounds up SAP Web Services and <a href="http://www.ruby-lang.org" target="_blank">Ruby</a> - you can either access them in Ruby directly by using the library sapwas, or taking advantage of the <a href="http://www.rubyonrails.org/" target="_blank">Rails</a> integration provided by sap4rails.
</p>

<div id="a000061more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at October 31, 2006 11:12 AM</p>





