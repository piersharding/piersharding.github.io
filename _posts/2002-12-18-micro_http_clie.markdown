---
layout: post
title:  "Micro HTTP Client for the command line"
date:   2002-12-18 12:00:00
categories: general
---


Just recently we had a requirement to have a standalone command line tool for HTTP
calls.... and I hear you say "what about lwp-request, lynx, links, wget, curl ..." - 
and you'd be right.  The difference with what we needed was that we had to get it
to run under LRP (<a href='http://www.linuxrouter.org'>The Linux Router
Project</a>), which is a minimalist Linux
distribution.  The second catch is that because the tool needed to be able to comply with 
a  highly RESTian web service, most of the tools currently available are difficult to use
or just can't cope with producing the full breadth of HTTP verbs - GET, HEAD, POST, PUT, 
DELETE etc.<br/>
what I have come up with is microhttp (with lots of help from Dan Drown) - it has no additional library requirements (outside
glibc2 - libc6 ), and should compile on all systems (using gcc).
It allows you to set the method, and additonal headers via command line switches, and
(if required) pipe the body of the request in via STDIN - all results are squirted out
to STDOUT.<br/>
microhttp can be found at: <a
href='http://www.piersharding.com/download/microhttp.tgz'>microhttp</a>.

<div id="a000009more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at December 18, 2002 12:25 PM</p>





