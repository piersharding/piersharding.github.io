---
layout: post
title:  "An amendment to the http/https proxy server in Perl"
date:   2002-09-07 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2002/09/creating_a_prox.html">&laquo; Creating a proxy server in Perl that handles SSL too</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2002/11/gnomevfs_for_pe.html">GnomeVFS for Perl &raquo;</a>

</p>

<h2>September  7, 2002</h2>

<h3>An amendment to the http/https proxy server in Perl</h3>

Further to my previous blog about creating an SSL enabled Perl proxy - I have
discovered that the SSL engine would not initialise correctly if a default
certificate and key could not be found.  To fix this I have set some variables
for this so that their location can be specified.  The updated program is
available <a href='/download/proxy.pl'>here</a>.
The whole concept of being able to inspect the conversational flow of a series
of HTTP requests has allready paid great dividends.  I have been using hte
proxy to pin point a character encoding issue at work where Apache issues a
Content-Type header for iso-8859-1, and the AxKit issues one for utf-8.  Now I
just need ot figure out how to switch the iso-8859-1 one off :-(.
Cheers.

<div id="a000015more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September  7, 2002  1:25 PM</p>





