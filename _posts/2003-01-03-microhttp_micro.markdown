---
layout: post
title:  "microhttp - Micro HTTP Client  becomes a library"
date:   2003-01-03 12:00:00
categories: general
---


I decided that I liked the simplicity of microhttp so much, that I have
created a library version out of it.  The library provides all the simplicity
of the command-line version for inclussion in HTTP::MHTTP.  Now I have a light
weight but fast set of functions for performing HTTP operations in Perl to do
things like:
<pre>
  http_init();
  http_add_headers(
                    'User-Agent' => 'HTTP-MHTTP1/0',
                    'Host' => 'localhost',
                    'Accept-Language' => 'en-gb',
                    'Connection' => 'Keep-Alive',
                  );
  http_reset();
  my $rc = http_call('GET', 'http://localhost/');
  warn "Status: ".http_status()."\n";
</pre>

microhttp can be found at: <a
href='http://www.piersharding.com/download/microhttp.tgz'>microhttp</a>.
HTTP::MHTTP can be found at: <a
href='http://www.piersharding.com/download/HTTP-MHTTP-0.01.tar.gz'>HTTP::MHTTP</a>.

<div id="a000005more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at January  3, 2003  4:12 PM</p>





