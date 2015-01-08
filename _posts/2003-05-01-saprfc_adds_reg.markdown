---
layout: post
title:  "SAP::Rfc adds Registered RFC calls from SAP to Perl"
date:   2003-05-01 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2003/03/perl_integratio.html">&laquo; Perl Integration with SAP</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2003/07/oscon_2003_inte.html">OSCON 2003 - Integrating SAP R/3 with Open Source Software and Open Protocols &raquo;</a>

</p>

<h2>May  1, 2003</h2>

<h3>SAP::Rfc adds Registered RFC calls from SAP to Perl</h3>

A major new release of SAP::Rfc for Perl is out on CPAN.  This new
version (revised to 1.12) adds in the Registered RFC functionality of librfc.
Registered RFCs are the ability to call out from SAP R/3 ABAP code
via the Gateway to external registered programs. In theory it should
now be possible for users of SAP::Rfc to access their favourite bit of
Perl code, or application server what-not, from within an R/3
System.

<p>
The following example demonstrates a simple replacement for the example
registered RFC program that SAP supply - rfcexec.  rfcexec allows
native system calls to be made on remote non-SAP hosts where the 
rfcexec program is running.
</p>

<pre>

  use SAP::Rfc;
  use SAP::Iface;
  use Data::Dumper;

  # construct the Registered RFC conection
  my $rfc = new SAP::Rfc(
                TPNAME   => 'wibble.rfcexec',
                GWHOST   => 'some.host',
                GWSERV   => '3300',
                TRACE    => '1' );

  # Build up the interface definition that the ABAP code is going to
  # call including the subroutine reference that will be invoked
  # on handling incoming requests
  my $iface = new SAP::Iface(NAME => "RFC_REMOTE_PIPE", HANDLER => \&do_remote_pipe);

  $iface->addParm( TYPE => $iface->RFCIMPORT,
                   INTYPE => $iface->RFCTYPE_CHAR,
                   NAME => "COMMAND",
                   LEN => 256);

  $iface->addParm( TYPE => $iface->RFCIMPORT,
                   INTYPE => $iface->RFCTYPE_CHAR,
                   NAME => "READ",
                   LEN => 1);

  $iface->addTab( NAME => "PIPEDATA",
                  LEN => 80);

  # add the interface definition to the available list of RFCs
  $rfc->iface($iface);

  # kick off the main event loop - register the RFC connection
  # and wait for incoming calls
  $rfc->accept();
  ...

  # the callback subroutine
  # the subroutine receives one argument of an SAP::Iface
  # object that has been populated with the inbound data
  # the callback must return "TRUE" or this is considered
  # an EXCEPTION
  sub do_remote_pipe {
    my $iface = shift;
    warn "Running do_remote_pipe...\n";
    my $ls = $iface->COMMAND;
    $iface->PIPEDATA( [ map { pack("A80",$_) } split(/\n/, `$ls`) ]);
    warn "   Data: ".Dumper($iface->PIPEDATA);
    return 1;
  }

</pre>

For further information SAP::Rfc can be found at: <a
href='http://search.cpan.org/search?author=PIERS'>SAP::Rfc</a>.

<div id="a000024more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at May  1, 2003 12:41 PM</p>





