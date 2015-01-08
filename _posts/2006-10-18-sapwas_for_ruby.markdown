---
layout: post
title:  "sapwas for Ruby and HTTPS"
date:   2006-10-18 12:00:00
categories: general
---


<p>
sapwas for Ruby now enables SAP Web Services to be called via HTTPS.  This first requires you to setup SSL support in NW4 - which isn't too much trouble if you follow Gregors' excellent advice <a href='https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/wlg/1452'>here</a>.  Once that is in place, then it is just a matter of structuring the URL coorectly, as described in this example:
</p>
<pre>
require "SAP/WAS"

@was = SAP::WAS.new(:url => "https://seahorse.local.net:8443/sap/bc/srt/rfc/sap/Z_RFC_READ_REPORT_01",
                    :lang   => "EN",
                    :client => "010",
                    :user   => "developer",
                    :passwd => "developer",
                    :trace  => true)

# get a list of users
irep = @was.RFC_READ_REPORT
irep.program.value = 'SAPLGRAP'
irep.call()
puts "qtab no rows are: #{irep.qtab.rows.length.to_s}"
puts "qtab rows are: #{irep.qtab.rows.inspect}"
</pre>

<p>
sapwas fully integrates with sap4rails as an alternative driver for accessing either RFCs or SAP  SOAP based Web Services, and is available <a href='http://raa.ruby-lang.org/project/sapwas/'>here</a>.
</p>

<div id="a000066more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at October 18, 2006 10:48 AM</p>





