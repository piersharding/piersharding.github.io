---
layout: post
title:  "New release for sapnwrfc PHP and Python"
date:   2009-08-27 12:00:00
categories: general
---


<p>Been a busy month, working on the NW SAP RFC connectors.  With build help from Menelaos, I now have a working Python build system for Windows on the Python NW RFC Connector as of version 0.07 - this is available <a href="http://www.piersharding.com/download/python/sapnwrfc/">here</a>.</p>

<p>Also, with help from Joachim, I've added a static function sapnwrfc_removefunction(&lt;sysid&gt;, &lt;function name&gt;) to the PHP connector that allows the removing of function definitions from the local cache.  this is most useful when developing RFC applications in PHP, as you can modify your RFC definition without having to restart the web server everytime.  This is available from version 0.09 <a href="http://www.piersharding.com/download/php/sapnwrfc/">here</a>.</p>

<div id="a000085more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at August 27, 2009  7:43 AM</p>





