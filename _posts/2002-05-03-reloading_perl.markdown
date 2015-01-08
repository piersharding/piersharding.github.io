---
layout: post
title:  "Reloading Perl Modules in a Server ( using Perl )"
date:   2002-05-03 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2002/05/examples_of_tab.html">&laquo; Examples of tables and SAP::Rfc</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2002/05/a_taste_for_inl.html">A taste for Inline &raquo;</a>

</p>

<h2>May  3, 2002</h2>

<h3>Reloading Perl Modules in a Server ( using Perl )</h3>

Just this week I have been working on a problem on how to reload a Perl module
in a continuous process eg. when you have changed the code in a loaded module,
you want the parent process to detect this, and then dump the old copy in
favour of the new.
<br/>
What I didn't know was that I would end up deep in Perl Guts, and that it
isn't as bad as it sounds.
<br/>
Scenario: we have an Application server that processes business functions.
This server is the back office for web servers that are possibly operational 24/7.
When we deploy new code we want to be able to do so with minimal down time
( hopefully none ), so just like with Apache you can use 
<a href='http://search.cpan.org/search?dist=Apache-Reload'>Apache::Reload</a>
to refresh mod_perl Modules, We wanted to be able to do the same.
<br/>
This boiled down to a couple of things:
<ul>
<li>
Register a Perl package with some controling agent to determine its baseline
modification date</li>
<li>Test the current timestamp of the modules file before each
incarnation/execution, and reload if necessary.</li>
</ul>

<h2>Baseline</h2>
The first thing I needed was the filename of the package, to do this I had to
turn the package name into a file name and then find that file in the
available paths in @INC.  when looping at @INC you have to be very careful not
to modify it, so I copied it first.
<pre>
  $pkg =~ s/::/\//g;
  $pkg .= '.pm';
  if ( -f $pkg ){
    $files->{$mod} = $pkg;
    return $pkg;
  } else {
    my @incy = ( @INC );
    foreach ( @incy ){
      if ( -f $_.'/'.$mod.'.pm' ){
        $files->{$mod} = abs_path($_).'/'.$mod.'.pm';
	  return $files->{$mod};
      }
    }
    return undef;
  }
</pre>

After that I need the to create a cache of the timestamp of the file
<pre>
  return (stat($file) )[9];
</pre>

<h2>Test and Reload</h2>
Now that I can locate the module and have a baseline to check against, I new
to check the timestamp repeatedly ( as above ), and then remove the module
from the symbol table, and reload it if the timestamp has been updated.

<pre>
  delete $INC{$file};
  my $pkg = $mod;

  # expand to full symbol table name if needed
  $pkg .= '::' unless  $pkg =~ /::$/;

  no strict "refs";
  my $symtab = *{$pkg}{HASH};
  return unless defined $symtab;

  # free all the symbols in the package
  foreach my $name (keys %$symtab) {
    debug("undef: $pkg$name");
    undef %{$pkg . $name};
  }

  # delete the symbol table
  undef %{"$mod"};
</pre>

As is often the case with Perl - here in lies the little bit of magic that
makes it all happen.  I delete the entry in the %INC hash that holds the directory
of loaded modules.  Then I figure out the symbol table entry for the package.
  With this name I can lookup the symbol table and find all the related entries 
  to the package ie. the names of all methods of the package.  these are:
<br/>
<span class='code'> undef %{$pkg . $name};</span> undefined, and then the
package as a whole is undefined.
<br/>
And that's it!  The code can be found in <a href='/download/Reload.pm.txt'>Reload.pm</a>
<br/>
All the credit goes to the respective authors of Apache::Session, and Symbol,
where I shamelessly plaigarised all the ideas from.  :-)

<div id="a000017more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at May  3, 2002  8:33 PM</p>





