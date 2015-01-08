---
layout: post
title:  "A taste for Inline"
date:   2002-05-14 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2002/05/reloading_perl.html">&laquo; Reloading Perl Modules in a Server ( using Perl )</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2002/07/oscon_2002_embe.html">OSCON 2002 - Embedding Perl with Inline &raquo;</a>

</p>

<h2>May 14, 2002</h2>

<h3>A taste for Inline</h3>

<a href='http://inline.perl.org'>Inline</a> is one of those potentially killer
apps for Perl.  It enables you to bind just about anything you feel like to
perl, and access it as native function calls from with your perl program.  It
started out as an idea to make it so compellingly easy to bind other
programming languages to Perl, that Perl could then take over the world ( okay
I'm starting to exagerate now ), but it has become more than that.  People are
now starting to wakeup to the idea of bind other things like SQL, and query
languages, through to text processing tools and the like <a
href='http://search.cpan.org/search?mode=module&query=Inline'> ... see this</a> .
<br/>
As a celebration, of what can be developed easily with Inline::C I dug out the
source code of the old UNIX command line tool <strong>cal</strong>, turned it
into a library, and bound it into a Perl module UNIX::Cal available at a CPAN
near you -  <a href='http://cpan.valueclick.com/authors/id/P/PI/PIERS/UNIX-Cal-0.01.tar.gz'>UNIX::Cal</a>

<div id="a000003more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at May 14, 2002  9:39 PM</p>





