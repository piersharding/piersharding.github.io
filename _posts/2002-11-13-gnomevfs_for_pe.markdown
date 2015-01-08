---
layout: post
title:  "GnomeVFS for Perl"
date:   2002-11-13 12:00:00
categories: general
---


I've just finished a early version of a virtual file system module for Perl based on
the <a href='http://gnome.hellocity.net/doc/API/gnome-vfs-2.0/index.html'>gnome-vfs</a>
library ( part of the Gnome desktop project ). Gnome VFS enables the access of of most 
universal URIs through a single API eg. http, https, file, ftp etc.<br/>
The VFS::GnomeVFS module will eventually be a driver module for the in progress VFS 
module, but can be used on it's own.  The module can still be used on it's own using a 
Perlish kind of interface - for example:<br/>
<pre>
use VFS::Gnome;
vfsopen(*FH, "<http://www.piersharding.com") or die $!;
my @lines = (<FH>);
close FH;
</pre>
VFS::Gnome can be found on CPAN at http://search.cpan.org/author/PIERS/.

<div id="a000004more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at November 13, 2002  5:42 PM</p>





