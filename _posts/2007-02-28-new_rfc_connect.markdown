---
layout: post
title:  "New RFC Connector - sapnwrfc"
date:   2007-02-28 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2006/11/rfc_string_and.html">&laquo; RFC_STRING and RFC_XSTRING type support for saprfc for Ruby</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2007/03/sap4rails_upgra.html">sap4rails updated to support new sapnwrfc  &raquo;</a>

</p>

<h2>February 28, 2007</h2>

<h3>New RFC Connector - sapnwrfc</h3>

<p>
SAP has undertaken a project to re-engineer the RFC SDK (creating the SAP NetWeaver RFC SDK), which is good news for the  Connectors (<a href="https://www.sdn.sap.com/irj/sdn/thread?forumID=50&threadID=286689">ref.</a>).</p>
<p>
This means that eventually - all the RFC connectors, and 3rd party products will need to be updated to use the new under-carriage.</p>
<p>
<h3>So whats the big deal?</h3>
RFC is a stable technology, and has been for many years, so I can understand why this revelation may not seem very exciting.  What is exciting is the unprecedented level of cooperation, understanding and good will that has come out in a relatively short time, as I have moved through the process of redeveloping the Ruby and Perl RFC Connectors.  The result is (and will be more so), a better fit in terms of how the SDK works with Dynamic Languages, allowing the API that the   Dynamic Languages offer for RFC connectivity, to better reflect the nature of those programming languages.  For example - there are better features in the new NW RFC SDK that allow for easy translation of ABAP types to Ruby/Perl types.
</p>
<p>
<h3>New Features</h3>
If we set aside the rationalisation, and simplification of the NW SDK (which is a bonus in itself), there are new features of the NW SDK that can be drawn upon -
<ul>
<li>unicode in the core, with utf-8 to utf-16 conversion tools</li>
<li>intelligent ABAP <=> X helper functions to ease type translations</li>
<li>deep structures</li>
<li>interface discovery</li>
<li>interface, and structure caching</li>
</ul>
</p>
<p>
This has lead to a complete overhaul of the Ruby, and Perl Connectors, with the aim to take advantage of the new NW SDK features, and to produce connectors that create a more intuitive bond between the underlying RFC API, and the natural features of each Dynamic Language.
</p>
<p>
<h3>Call for testers</h3>
As the new Ruby and Perl RFC Connectors are a complete rewrite and Alpha, I am calling for testers/early adopters from the Community.
</p>
<p>
<h3>Obtaining the Connectors, and Netweaver RFC SDK</h3>
Download the new sapnwrfc connector for Ruby, and get the new RFC SDK, port your applications to it, and let me know how you get on.  I'm interested in usability feedback, problems, and feature requests.
</p>
<p>
For Ruby - download from the <a href="http://raa.ruby-lang.org/project/sapnwrfc/" target="_blank"> RAA</a>.  Follow the instructions in the included README file.  Documentation is available <a href="http://www.piersharding.com/download/ruby/sapnwrfc/doc/" target="_blank">Here</a>.  A GEM install package for Win32 has been built available <a href="http://www.piersharding.com/download/ruby/sapnwrfc/sapnwrfc-0.06-mswin32.gem">here</a>.
</p>
<p>
For Perl - download from <a href="http://search.cpan.org/search?query=sapnwrfc" target="_blank">CPAN</a>.  Again - follow the instructions in the included README file.  Documentation is available <a href="http://search.cpan.org/author/PIERS/sapnwrfc-0.06/sapnwrfc.pm" target="_blank">Here</a>.  A PPM install package for Win32 has been built available <a href="http://www.piersharding.com/download/win32/sapnwrfc-0.06.zip">here</a>.
</p>
<p>
If you are interested in trialling/testing the new connectors, then along with the installing the new connectors (above) you will need to obtain the new Netweaver RFC SDK.  in order to do this, please register your interest with me, ensuring that I know how to contact you, and what your platform requirements are, and with the help of the NW RFC team at SAP I will get the relevant details to you.
</p>
<p>
<h3>Ruby Examples</h3>
There are plenty of examples in the tests/ directory of the sapnwrfc download, but here is a basic walk through of the new API:
<pre>
 # specify a YAML base config file or pass connection
 #   parameters directly to rfc_connect()
 SAPNW::Base.config_location = './config_file.yml'
 SAPNW::Base.load_config
 conn = SAPNW::Base.rfc_connect

 # get the system and connection details
 attrib = conn.connection_attributes
 $stderr.print "Connection Attributes: #{attrib.inspect}\n"

 # lookup the dictionary definition of an Function Module
 fds = conn.discover("STFC_DEEP_STRUCTURE")
 $stderr.print "Parameters: #{fds.parameters.keys.inspect}\n"

 # create an instance of a Function call
 fs = fds.new_function_call

 # populate the parameters - structures and table rows now take hashes of field name/value pairs
 fs.IMPORTSTRUCT = { 'I' => 123,
                     'C' => 'AbCdEf',
                     'STR' =>  'The quick brown fox ...',
                     'XSTR' => ["deadbeef"].pack("H*") }

 # execute the RFC call
 fs.invoke
 $stderr.print "RESPTEXT: #{fs.RESPTEXT.inspect}\n"
 $stderr.print "ECHOSTRUCT: #{fs.ECHOSTRUCT.inspect}\n"
</pre>
</p>
<p>
Config file (refer to the sap.yml file in the download):
<pre>
ashost: ubuntu.local.net
sysnr: "01"
client: "001"
user: developer
passwd: developer
lang: EN
trace: 2
</pre>
</p>
<p>
Test it out - and give your feedback.
</p>
<p>
<h3>Perl Examples</h3>
As with Ruby, there are plenty of examples in the software download in the t/* directory.  Again - here is a taster showing the new API:
<pre>
 use sapnwrfc;
 use Data::Dumper;

 # specify a YAML base config file or pass connection
 #   parameters directly to rfc_connect()
 SAPNW::Rfc->load_config;
 my $conn = SAPNW::Rfc->rfc_connect;

 # lookup the Function Module
 my $fds = $conn->function_lookup("STFC_DEEP_STRUCTURE");

 # initialise a call instance
 my $fs = $fds->create_function_call;

 # set the parameters
 $fs->IMPORTSTRUCT({ 'I' => 123,
                     'C' => 'AbCdEf',
                     'STR' =>  'The quick brown fox ...',
                     'XSTR' => pack("H*", "deadbeef")});

 # invoke the Function Module and then play with the results
 $fs->invoke;
 $stderr.print "RESPTEXT: #{fs.RESPTEXT.inspect}\n"
 $stderr.print "ECHOSTRUCT: #{fs.ECHOSTRUCT.inspect}\n"
 print STDERR "RESPTEXT: ".Dumper($fs->RESPTEXT)."\n";
    ok($c eq 'AbCdEf');
 print STDERR "ECHOSTRUCT: ".Dumper($fs->ECHOSTRUCT)."\n";

 # cleanup
 $conn->disconnect;
</pre>
</p>
<p>
Config file format is the same as for Ruby - refer to the sap.yml file in the download:
<pre>
ashost: ubuntu.local.net
sysnr: "01"
client: "001"
user: developer
passwd: developer
lang: EN
trace: 2
</pre>
</p>
<p>
Test it out - and give your feedback.  The best place would be to carry on the discussion through the <a href="https://forums.sdn.sap.com/category.jspa?categoryID=39">Forums</a>.
</p>
<p>
Special thanks go to:
<ul>
<li> Ulrich Schmidt for fielding all my questions, and the whole of the NW RFC Team (NW AS ABAP Connectivity), for their support, and quick response.</li>
<li>Olivier Boudry - building and testing</li>
<li>and lastly (but by no means least) Craig Cmehil for tirelessly getting people together and making it all possible.</li>
</ul>
</p>

<p>
<a href=""><h3>Notes/Updates:</h3></a>
See the download instructions for the SAP NW RFCSDK  <a href='http://www.piersharding.com/blog/archives/2007/05/nw_rfc_sdk_is_n.html'> here</a>.
</p>

<div id="a000071more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at February 28, 2007 12:28 PM</p>





