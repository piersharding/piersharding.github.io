---
layout: post
title:  "Calling Perl from Java"
date:   2004-04-17 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2003/12/ruby_saprfc_add.html">&laquo; Ruby saprfc adds Registered RFC calls from SAP R/3</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2004/04/release_125_sap.html">Release 1.25 - SAP::Rfc for Perl &raquo;</a>

</p>

<h2>April 17, 2004</h2>

<h3>Calling Perl from Java</h3>

Something that I have been playing with of late is calling Perl from
Java, and it is something that I can see people needing more and more as
time goes by.<br/>
<pre>
Java-|
     |--> Perl (and maybe back again at some later date)
</pre>
After talking with Patrick Leboutillier of <a
href='http://search.cpan.org/search?query=Inline%3A%3AJava'>Inline::Java</a>, I decided to
have a crack at building this functionality myself (at the time
Inline::Java&apos;s focus was on calling Java from Perl, and the reverse was
not complete).<br/>

What I needed was to be able to communicate with a custom built Perl
based application server that we use to abstract our business logic from
our Business Portal, to <a href='http://www.sap.com'>SAPs</a> new J2EE
 enable server technology.
<br/>
If anyone is interested in having a go, the results of this so far is <a
href='/download/javaperl.0.1.tar.gz'>org.perl.java</a>.<br/>
The README explains the difficulties with JVMs, and the build
requirements of Perl.  So far, there is the ability to load an initial
perl program, eval an arbitrary piece of Perl, call a static Perl
method, and do an instance method call, on a previously defined Perl
object reference.  All return values are either further Perl object
references or strings currently.

<div id="a000019more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 17, 2004  9:43 PM</p>





