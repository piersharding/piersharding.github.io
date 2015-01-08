---
layout: post
title:  "Jabber::mod_perl 0.15 released"
date:   2005-12-30 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2005/12/jabbermod_perl_1.html">&laquo; Jabber::mod_perl 0.10 released</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/03/builiding_perl.html">Builiding Perl SAP::Rfc for win32 &raquo;</a>

</p>

<h2>December 30, 2005</h2>

<h3>Jabber::mod_perl 0.15 released</h3>

<p>
Version 0.15 brings a more complete process for creating new packets, and more complete Namespace handling, especially in the area of searching for elements within a NAD.<br/>
Highlights from the changes file include:<ul>
  <li> Corrected problem with $nad->find_scoped_namespace() where an empty prefix was not recognised.</li>
  <li>Fixed $nad->elem() so that it now correctly finds an element given the context of a namespace.</li>
  <li>Added $pkt->create() - this enables the creation of a new packet in the context of the current packets session.</li>
</ul>
</p>
<p>
     Refer to the examples/MyStamp.pm for a good overview of packet handling and creation.
</p>
<p>
Download from <a href='http://search.cpan.org/~piers/Jabber-mod_perl-0.15/'>here</a>.  Any feedback welcome.
</p>

<div id="a000047more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at December 30, 2005  4:24 PM</p>





