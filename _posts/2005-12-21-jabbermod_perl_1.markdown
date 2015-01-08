---
layout: post
title:  "Jabber::mod_perl 0.10 released"
date:   2005-12-21 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2005/11/sap_and_open_so.html">&laquo; SAP and Open Source: an analysis and letter to SAP and Shai</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2005/12/jabbermod_perl_2.html">Jabber::mod_perl 0.15 released &raquo;</a>

</p>

<h2>December 21, 2005</h2>

<h3>Jabber::mod_perl 0.10 released</h3>

Lately, I have been doing quite a bit more work on Jabber::mod_perl, after leaving it for a while.  Things are starting to heat up in the Jabber world, and Perl threading/unicode support has matured a bit, which is bring it back into focus again.
<br/>
<p>
This release continues to iron out some bugs with NAD handling, and
gives some much needed features for changing packets as they pass
through:
<ul>
  <li> Added nad_replace_cdata_head(), and nad_replace_cdata_tail().
    These complement the nad_append_cdata* methods.</li>
   <li> added support for nearly all the module chain events - support
   list is now:
<ul>
    <li> sess-start</li>
    <li> sess-end</li>
    <li> in-sess</li>
    <li> out-sess</li>
    <li> in-router</li>
    <li> out-router</li>
    <li> pkt-sm</li>
    <li> pkt-user</li>
    <li> pkt-router</li>
  </li>
</ul>
</ul>
</p>
<p>
  Please refer to the sample modules in the examples subdirectory, and the
sm.xml file for the config.
</p>
<p>
Download from <a href='http://search.cpan.org/~piers/Jabber-mod_perl-0.10/'>here</a>.  Any feedback welcome.
</p>

<div id="a000046more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at December 21, 2005 11:48 AM</p>





