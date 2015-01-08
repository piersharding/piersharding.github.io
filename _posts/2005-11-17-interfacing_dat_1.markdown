---
layout: post
title:  "Interfacing data into BW using Perl, Ruby, or Python"
date:   2005-11-17 12:00:00
categories: general
---


<p>
I've just completed the first SAP related course that I've been on for 
many years - BW310 - in London, Clockhouse Place.  Surprisingly, I have
really enjoyed it, and the lecturer has been excellent (my thanks go to Gurjeet
Dosanjh).
</p>
<p>
On the back this, and other fiddling with the Business Warehouse, I've had a
look at loading data in "push" mode, and have struck yet another great use for
RFC support available in scripting languages.  With the unrivaled abilities of scripting languages to
munging, mash, and manipulate any kind of data, coupled with  the automatically generated RFC interface
(DataSource) attached to an InfoSource within BW, the job is trivial.
</p>
<p>
The RFC DataSource piggy-backs on the BW <-> XI SOAP interface that is generated when you go to
edit your InfoSource in the AWB (transaction RSA1).  This is accessed by selecting the Extras menu,
and then choosing "Create BW DataSource with SOAP Connection".
</p>
<img src="https://weblogs.sdn.sap.com/weblogs/images/8630/bw_rfc_1.1.png" width="600" height="376" border="0" alt="image" />
<i>Edit InfoSource view - automatically generating the SOAP RFC connector</i>
<p>
This is discussed in detail on help.sap.com at
 <a href="http://help.sap.com/saphelp_nw04/helpdata/en/9b/821140d72dc442e10000000a1550b0/content.htm"> this page</a>

You must pay carefull attention to the details surrounding  "Creating XML
DataSources" and "Activating Data Transfer to the Delta Queue".  You can check if
you have this right by looking at transaction RSA7 - BW Delta Queue
Maintenance, and making sure the traffic lights are green.
</p>
<p>
Once you have activated the RFC interface, you need to determine what the real "technical name is".  
The technical name for the DataSource that I created for the RFC interface
example below is 6AZODSINFOXX (as viewed in RSA7) - this translates to an RFC name of
/BI0/QI6AZODSINFOXX_RFC - just open yours up in SE37 for the details.
</p>
<p>
The following example is written in <a href="http://www.ruby-lang.org">Ruby</a>, however, the language bindings exist for RFC for Ruby, Perl, and Python, so - please - choose what you fancy - you can find out more about them all at <a href="http://www.piersharding.com/blog/">here</a> or look at previous articles that I have written  <a href="https://www.sdn.sap.com/irj/sdn/weblogs?blog=/pub/u/8630">here</a>.
</p>
<p>
Here is a basic step through of what it takes to get the data loading scenario
going:
</p>
<p>
<pre style='border-style : solid ; border-width: 1px ; border-color: #000000 ;'>
require "SAP/Rfc"

# connect
rfc = SAP::Rfc.new(:ashost => "seahorse.local.net",
                   :sysnr  => 00,
                   :lang   => "EN",
                   :client => "010",
                   :user   => "developer",
                   :passwd => "developer",
                   :trace  => 1)

# lookup the BW RFC interface
i = rfc.discover("/BI0/QI6AZODSINFOXX_RFC")

# set the datasource to connect to
i.datasource.value = "6AZODSINFOXX"

# grab the structure of the transfer structure
str = i.data.structure

# start processing the input file - doesnt have to be a file  though
fh = File.open("./data1.csv")
i.data.value = []

fh.each { |line|
  # drop the first line
  next if fh.lineno == 1

  # remove the line end
  line.chomp!

  # split the columns on ","
  flds = line.split(",")

  # remove any quotes around column values
  flds.each {|f| f.sub!(/^\"(.*?)\"$/, '\1') }

  # fillout the BW transfer structure
  str.getField("RECORDMODE").value = flds[0]
  str.getField("/BIC/ZEQUIP_XX").value = flds[1]
  str.getField("/BIC/ZNAME_XX").value = flds[2]

  # add the row to the table
  i.data.value.push(str.value)
}
fh.close

# call and test the return
rfc.call(i)

# basic error checking of the inserted record batch - check the exception thrown, if any
print "Error is: #{i.error}\n"
</pre>
</p>
<p>
In this example I have just used a typical CSV input file as  displayed below to
provide input data - but you can use your imagination here - it could be <a href="http://www.mysql.com">MySQL</a>, a <a href="http://poe.perl.org">POE</a> server, your legacy AS400 system - it's up to you :-)

File:
</p>
<p>
<pre style='border-style : solid ; border-width: 1px ; border-color: #000000 ;'>
Recmode,Equipment,Name
,0000009011,"CONTROLS FAILED - 3"
,0000009012,"VALVE LEAK - 3"
,0000009013,"OIL LEAK - 3"
,0000009014,"NUT WIBBLED - 3"
,0000009012,"TYRE WOBBLES - 3"
,0000009011,"NUT FAILURE - 3"
</pre>
</p>
<p>
Enjoy! :-)
</p>

<div id="a000044more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at November 17, 2005  7:15 AM</p>





