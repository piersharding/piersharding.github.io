---
layout: post
title:  "Changing parameters in saprfc for Ruby"
date:   2006-10-12 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2006/10/saprfc_and_sapr_1.html">&laquo; SAP::Rfc and saprfc UNICODE support for win32</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/10/ruby_ruby_on_ra.html">Ruby, Ruby on Rails, and SAP Web Services Integration &raquo;</a>

</p>

<h2>October 12, 2006</h2>

<h3>Changing parameters in saprfc for Ruby</h3>

<p>saprfc for Ruby now has the ability to pass Changing type parameters.  These are an evolution of import, and export parameter types rolled into one.  this functionality is available from release 0.31.  This example shows the standard test RFC where COUNTER is a changing type parameter - the value is incremented on each pass:</p>
<pre>
iter = 10
rfc = SAP::Rfc.new(...)
# look up the interface definition for an RFC
i = rfc.discover("STFC_CHANGING")
i.counter.value = 0
iter.times {|cnt|
  puts "\n\n\n\n\nITERATION #{cnt + 1}\n\n\n\n"
  i.start_value.value = cnt
  rfc.call(i)
  puts "RESULT: #{i.result.value}  COUNTER: #{i.counter.value}"
}
# close the connection
print "close connection: ", rfc.close(), "\n"
</pre>

<div id="a000065more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at October 12, 2006  6:15 AM</p>





