---
layout: post
title:  "RFC_STRING and RFC_XSTRING type support for saprfc for Ruby"
date:   2006-11-11 12:00:00
categories: general
---


<p>Support for string types now available in saprfc for Ruby from version 1.52.  This opens the way for interaction with RFC calls that require variable length storage eg. true strings in either character or binary form.  This was particularily useful for manipulating logon tickets, as shown by this example:
</p>
<p>
<pre>
isusr = rfc.discover("SUSR_CHECK_LOGON_DATA")
isusr.AUTH_METHOD.value = "E"
isusr.AUTH_DATA.value = "p:ompka\\piers"
isusr.EXTID_TYPE.value = "NT"
rfc.call(isusr)
puts "RESULT: "

# access an interface parameter value
print "TICKET:             #{isusr.TICKET.value.to_s}\n"
ticket = isusr.TICKET.value.to_s

rfc2 = SAP::Rfc.new(:ashost => "192.168.1.2",
                   :sysnr  => 00,
                   :lang   => "EN",
                   :client => "010",
                   :mysapsso2 => ticket,
                   :trace => 1
       )
</pre>
</p>
<p>
This code snippet demonstrates using one RFC connection to generate login tickets for another - a very useful trick for brokering connections within an external application.  The login tickets in the standard RFC are carried in an RFC_STRING (isusr.TICKET) type parameter.
</p>
<p>Note: Thanks to <a href='http://www.computerservice-wolf.com/'>Gregor</a> for pointing this out.</p>

<div id="a000064more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at November 11, 2006 11:00 AM</p>





