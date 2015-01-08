---
layout: post
title:  "Unicode support for Perl SAP::Rfc and Ruby saprfc"
date:   2006-08-12 12:00:00
categories: general
---


I am pleased to announce some support for UNICODE in Perl SAP::Rfc and  Ruby saprfc.  This is a major step forward, as it incorporates for the first time the use of the SAP supplied Unicode RFC library - librfcu*.<br/>
Please download <a href='http://search.cpan.org/search?dist=SAP-Rfc'>SAP::Rfc 1.45  for Perl</a> and <a href='/download/ruby/saprfc-0.21.tar.gz'>saprfc-0.21 for Ruby</a> and follow the build instructions in the README file.  Make sure that you retrieve u16lit.pl form http://service.sap.com, and the appropriate rfcsdk containing the unicode libraries from SAP.
<br/>
The unicode support is restricted to Client side RFC at the moment, as it is a completely separate task to replicate this for Registered RFC server programs.

<div id="a000054more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at August 12, 2006  7:30 PM</p>





