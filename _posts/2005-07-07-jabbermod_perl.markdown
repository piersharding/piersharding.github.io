---
layout: post
title:  "Jabber::mod_perl 0.06 released"
date:   2005-07-07 12:00:00
categories: general
---


<p>
Announcing <a href='http://search.cpan.org/dist/Jabber-mod_perl/'>Jabber::mod_perl 0.06</a>.
</p>
<p>
My thanks go out to Christopher (and the whole <a href='http://jabberstudio.org/projects/jabberd2/project/view.php'>jabberd</a> development team)
for his patches for Jabber::mod_perl.  Nice to know that  it is being
used.
</p>
<p>
Changes rolled out this time are:
<ul>
     <li> applied patches supplied by Christopher Zorn to make the build work for <a href='http://jabberstudio.org/projects/jabberd2/releases/'>j2.0s8</a></li>
     <li> fixed an old bug with build where make was picking up Perls config.h instead of jabberd2s</li>
     <li> for the moment - I have removed jadperl from the build, and concentrated on mod_perl for sm - my thinking is that there are so many Perl implementations for building components and clients that I should focus on the core functionality of being able to run Perl code from within the server.  If this causes a problem, then let me know.</li>
</ul>

<div id="a000034more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at July  7, 2005  9:43 AM</p>





