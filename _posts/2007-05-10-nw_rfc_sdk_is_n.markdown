---
layout: post
title:  "NW RFC SDK is now officially available"
date:   2007-05-10 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2007/03/sap4rails_upgra.html">&laquo; sap4rails updated to support new sapnwrfc </a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2007/05/sapnwrfc_for_py.html">sapnwrfc for Python &raquo;</a>

</p>

<h2>May 10, 2007</h2>

<h3>NW RFC SDK is now officially available</h3>

<h2>The new SAP NW RFCSDK</h2>
<p>
The new SAP NetWeaver RFCSDK is now available for official download - this opens the way for supported next generation Open Source Connectors such as <a href="http://search.cpan.org/search?dist=sapnwrfc" target="_blank">sapnwrfc</a> for <a href="http://www.perl.org" target="_blank">Perl</a>, and <a href="http://raa.ruby-lang.org/project/sapnwrfc" target="_blank">sapnwrfc</a> for <a href="http://www.ruby-lang.org" target="_blank">Ruby</a>.
</p>
<h2>Where to get it?</h2>
<p>
You need to go to the SAP service Portal <a href="http://service.sap.com/swdc">for Software downloads</a>, and follow the path of:
Download -&gt; Support Packages and Patches                                          
-&gt; Entry by Application Group -&gt; Additional Components  -&gt; SAP NW RFC                                            
SDK -&gt; SAP NW RFC SDK 7.10 -&gt; SAP NW RFC SDK 7.10                  .
</p>
<p>
There is an accompanying   OSS note:    <a href="https://service.sap.com/sap/support/notes/1025361" target="_blank">                                      
 1025361</a>.
</p>
<p>
<h2>Impact on Connectors</h2>
As mentioned in a 
<a href="https://weblogs.sdn.sap.com/cs/weblog/view/wlg/5827" target="_blank">previous blog</a> (but so much better in Ulrichs' <a href="https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/wlg/6528" target="_blank">blog</a>), the new NW RFCSDK not only updates the implementation of the library that all 3rd party products use, but it also provides a better interface to develop to, and exposes several key features such as Unicode, and complex structures.</p>
<p>
Clearly - this is SAPs way forward in the RFC area of closely bound system integration, so all existing connectors  (and 3rd party products) will eventually have to migrate to it.
</p>
<p>
This is also true of code that is based on the existing connectors for Perl and Ruby.  For Perl - there will be a migration from SAP::Rfc -&gt; sapnwrfc, and likewise for Ruby moving from saprfc -&gt; sapnwrfc  (The client side implementation for the new sapnwrfc connector for Python is nearly finished, with the same implications).
</p>
<p>
My thanks and congratulations go out to the SAP NW RFC Team (NW AS ABAP Connectivity) - through their efforts, and willingness to engage the community, they have made it possible for us frustrated hackers to have a little more fun :-)
</p>

<div id="a000073more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at May 10, 2007  8:49 AM</p>





