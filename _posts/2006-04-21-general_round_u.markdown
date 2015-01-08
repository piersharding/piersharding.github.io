---
layout: post
title:  "General round up of SAP RFC releases"
date:   2006-04-21 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2006/04/saprfc_015_for.html">&laquo; SAP::Rfc 0.15 for Ruby</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/04/radrails_rails.html">RadRails, Rails, Ruby, and SAP &raquo;</a>

</p>

<h2>April 21, 2006</h2>

<h3>General round up of SAP RFC releases</h3>

<p>
Things have been hotting up lately with my three RFC connectors for SAP using  Perl, Python and Ruby.
</p>
<p>
<h3>In brief:</h3>
</p>
<p>
<h3>Perl and SAP::Rfc 1.42</h3>
<ul>
<li>Support for SSO2 login has been added</li>
</ul>
The new version can be found on <a href="http://search.cpan.org/search?dist=SAP-Rfc" target="_blank">CPAN</a>
</p>
<p>
<h3>Ruby and SAP::Rfc 0.16</h3>
<ul>
<li>Support for SSO2 login has been added</li>
<li>Caching of interface, and structure discovery - whenever a call to SAP::Rfc#discover is made, a series of RFC calls are performed to look up what the definition of the interface is (same applies for SAP::Rfc#structure). This can be a performance hit for a complex RFC interface, or situations where an RFC enabled script is fired frequently. invoke SAP::Rfc.useCache = true to activate caching - see documentation for further details.</li>
<li>Inorder to tidy up accessors for parameters, and structure components, some changes have been made. The new syntax for setting parameter values is iface.PARM_NAME = value instead of iface.PARM_NAME.value = new_value. The same applies to structure components where: str.FIELD_NAME = new_value as opposed to str.FIELD_NAME.value = new_value.</li>
</ul>
The new version can be found at: <a href="http://www.piersharding.com/download/ruby/" target="_blank">saprfc</a> or via <a href="http://raa.ruby-lang.org/project/saprfc" target="_blank">RAA</a>
</p>
<p>
<h3>Python and saprfc 0.08</h3>
<ul>
<li>Support for SSO2 login has been added</li>
<li>Improved connection parameter handling</li>
</ul>
<a href="http://www.python.org/pypi?%3Aaction=search&name=saprfc&version=&summary=&description=&keywords=&_pypi_hidden=0" target="_blank">saprfc for Python</a> is on PyPi ("the Cheese Shop").
</p>


<div id="a000050more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 21, 2006 12:23 PM</p>





