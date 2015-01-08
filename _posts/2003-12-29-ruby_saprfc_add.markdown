---
layout: post
title:  "Ruby saprfc adds Registered RFC calls from SAP R/3"
date:   2003-12-29 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2003/07/oscon_2003_inte.html">&laquo; OSCON 2003 - Integrating SAP R/3 with Open Source Software and Open Protocols</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2004/04/calling_perl_fr.html">Calling Perl from Java &raquo;</a>

</p>

<h2>December 29, 2003</h2>

<h3>Ruby saprfc adds Registered RFC calls from SAP R/3</h3>

I finally found a  few spare moments to catch the Ruby saprfc implementation
up with the Perl one.<br/>
saprfc for Ruby now has support for registered RFCs, enabling Ruby programmers
to call out to their favourite language from within SAP ABAP code.
<p>
The following example demonstrates a simple replacement for the example
registered RFC program that SAP supply - rfcexec.  rfcexec allows
native system calls to be made on remote non-SAP hosts where the 
rfcexec program is running.
<br/>
On the SAP R/3 side of things, you need to create the relevent registered RFC
connectoid, by going to transaction SM59 "Display and Maintain RFC
Destinations".  Create a new TCP/IP connection, set the activation type to
"registered server program", and then enter the Registered Server Program -
Program ID.  In this case it is "wibble.rfcexec" (you choose something better
:-).
</p>

<pre>
require "lib/SAP/Rfc"

rfc = SAP::Rfc.new("", "", "", "", "", "", 1, "wibble.rfcexec", "localhost",
"3318")

iface = SAP::Iface.new("RFC_REMOTE_PIPE")
iface.addParm( SAP::Parm.new("COMMAND", nil, RFCIMPORT, RFCTYPE_CHAR, 256) )
iface.addParm( SAP::Parm.new("READ", nil, RFCIMPORT, RFCTYPE_CHAR, 1) )
iface.addParm( SAP::Tab.new("PIPEDATA", nil, 80) )

iface.callback = Proc.new  do |iface|
   call = `#{iface.COMMAND.value()}`
   call.split(/\n/).each do |val|
     iface.PIPEDATA.value.push(val.ljust(80))
   end
end

rfc.iface(iface)

rfc.accept()

</pre>

<p>
In order to test this you need to create a test ABAP program in SAP R/3 - the
code below corresponds to the example:
</p>

<pre>
REPORT  ZPXH1                                   .
parameters: cmd(80) lower case default 'ls -latr'.
data:
   begin of pipedata occurs 0,
     row(80),
   end of pipedata.

CALL FUNCTION 'RFC_REMOTE_PIPE' destination 'WIBBLE.RFCEXEC'
  EXPORTING
    COMMAND       = cmd
    READ          = 'X'
* IMPORTING
*   SYSTEM        =
*   TRDIR         =
  TABLES
    PIPEDATA      = PIPEDATA
          .
loop at pipedata.
  write: /01 pipedata.
endloop.
</pre>

<p>
Happy SAPing :-)
</p>

Further information on saprfc can be found at: <a
href='http://raa.ruby-lang.org/list.rhtml?name=saprfc'>SAP::Rfc</a>.

<div id="a000025more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at December 29, 2003 11:04 PM</p>





