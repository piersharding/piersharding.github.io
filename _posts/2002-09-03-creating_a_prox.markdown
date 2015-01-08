---
layout: post
title:  "Creating a proxy server in Perl that handles SSL too"
date:   2002-09-03 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2002/07/oscon_2002_embe.html">&laquo; OSCON 2002 - Embedding Perl with Inline</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2002/09/an_amendment_to.html">An amendment to the http/https proxy server in Perl &raquo;</a>

</p>

<h2>September  3, 2002</h2>

<h3>Creating a proxy server in Perl that handles SSL too</h3>

<a href='http://www.pipetree.com/qmacro/'>DJ</a> got to talking about HTTP
proxies, and how they are important.  He also mentioned the usefulness of
being able to inspect the transaction going backwards and forwards between the
client and server (headers, content etc.).  We also discussed the possibility
of writing one in Perl.  After 5 minutes on <a
href='http://www.google.com'>Google</a> sure enough, I found one from non
other than <a
href='http://www.stonehenge.com/merlyn/WebTechniques/col34.html'>Randal</a>
from a column he wrote several years ago.  This almost exactly suited my
purpose, except that as most websites do, ours has some pages covered by SSL.
Herein lay the challenge.  I couldn't find an example of a Perl based proxy
server that covers both HTTP and HTTPS, so I took Randals' example and
modified it to <a href='/download/proxy.pl'>this</a>.  The really interesting
thing is finding out how browsers and clients in general can vary so widely in
how they implement SSL.  lwp-request and Konqueror allow both http and direct
https SSL proxying.  Mozilla and IE don't (they implement the CONNECT palava) etc.<br/>
The discussion also went arround the fact that you can't actually "proxy" SSL and inspect it without breaking the chain so to speak - where the proxy actually negotiates certificates with the client instead of the client negotiating with the end target.

<div id="a000014more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September  3, 2002  5:59 AM</p>





