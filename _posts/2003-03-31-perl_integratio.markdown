---
layout: post
title:  "Perl Integration with SAP"
date:   2003-03-31 12:00:00
categories: general
---


<table width="100%">
<tr>
<td width="10"><img src="img/pix.gif" 
	width="10" height="0" border="0" vspace="0" hspace="0" 
	alt="" /></td>
<td width="*" style="font-size: small;">
<h1>SAP::Rfc and Friends</h1><p>Author: 
		Piers
		 Harding</p>

		
	Copyright &copy; 
		<year>2002</year>
		&nbsp;<a href="mailto:piers@ompa.net">Piers Harding</a>
	<p/>
<p>
   Perl integration with SAP via RFC, Business Connector, and Web Application Server 
</p>
	

<hr>


			<h1>Perl intergration with SAP</h1>

		
			<h2>SAP::Rfc - Perl intergration with SAPs'
			Native RFC library</h2>

<p>
  The latest
version of SAP::Rfc is available on CPAN.  This release uses the Inline::MakeMaker support for making installable modules, so now make, make test, and make install should work correctly ;-).
This is available for download at <i><a href="http://search.cpan.org/search?dist=SAP-Rfc">SAP::Rfc [1]</a></i>
This package taps into the native RFC library supplied by SAP and
binds it into Perl creating a clean object oriented interface for
performing RFC calls to an SAP R/3 system ( version 3.1+ - may work
for earlier versions but hasn't been tested ).
</p>
<p>
This package contains  examples ( check out the examples
directory ) ranging from extracting source
code from an SAP system to calling sales order IDOCS via RFC.
</p>
<p>
Periodically I get queries on how to do certain things with the SAP::Rfc library, 
  so I have started collecting the notes together:
  <ul>
    <li><i><a href="/article.xml?sapexamples">How To load and unload Tables in RFC calls [2]</a></i></li>
  </ul>
</p>

		
		
 <h2>SAP::BC::XMLRFC</h2>
<p>
The latest version of <i><a href="http://search.cpan.org/search?dist=SAP-BC-XMLRFC">SAP::BC::XMLRFC [3]</a></i> is available on CPAN.  This package uses LWP to do HTTP calls ( and with a few simple
modifications could do HTTPS calls ) to the SAP Business Connector to
do XML based RFC calls.  This is the SAP equivalent to SOAP, and
XML-RPC.
Similar to SAP::Rfc, this package provides a clean Object Oriented
interface to building RFC queries, and includes Inteface lookup
abilities to simplify interface definition.
			</p>
	
		
 <h2>SAP::WAS::SOAP</h2>
<p>
The latest version of <i><a href="http://search.cpan.org/search?dist=SAP-WAS-SOAP">SAP::WAS::SOAP [4]</a></i> is available on CPAN.  This package uses <i><a href="http://soaplite.com">SOAP::Lite [5]</a></i> to do SOAP calls to the SAP Web Application Server ( WAS ).
Similar to SAP::Rfc, this package provides a clean Object Oriented
interface to building RFC queries, and includes Inteface lookup
abilities to simplify interface definition.
			</p>
	
	<br/><!-- hr/ -->

<h1>List of Links</h1>
<table border="1">
<th>URL</th>

<tr>
<td>[1] http://search.cpan.org/search?dist=SAP-Rfc</td>
</tr>

<tr>
<td>[2] /article.xml?sapexamples</td>
</tr>

<tr>
<td>[3] http://search.cpan.org/search?dist=SAP-BC-XMLRFC</td>
</tr>

<tr>
<td>[4] http://search.cpan.org/search?dist=SAP-WAS-SOAP</td>
</tr>

<tr>
<td>[5] http://soaplite.com</td>
</tr>

</table>
</td>
</tr>
</table>

<div id="a000023more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at March 31, 2003  7:36 PM</p>





