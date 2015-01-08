---
layout: post
title:  "SAP::Rfc for Perl version 1.38"
date:   2005-08-23 12:00:00
categories: general
---


SAP::Rfc - SAP RFC - for Perl version 1.38 is now available on CPAN for download
<a href='http://search.cpan.org/search?dist=SAP-Rfc'>here</a>.  This is a minor bug release correcting a problem with structured parameters.

<div id="a000039more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at August 23, 2005 11:40 AM</p>




<h2 id="comments">Comments</h2>

<div id="c27">
<p>works perfect while accessing a non-unicode R3 System</p>

<p>when i am trying to fetch structured content from an unicode system the whole data is send in the first column and the column size is doubled...</p>

<p>it seems to be the same problem like in php project saprfc </p>

<p>i found a thread at sdn.sap.com concerning this problem (http://forums.sdn.sap.com/thread.jspa?threadID=43603)</p>

<p></p>

<p><br />
</p>
</div>
<p class="posted">Posted by: Bjoern  at September 15, 2005  3:50 PM</p>
<div id="c28">
<p>Hi Bjoern,</p>

<p>Nice catch - yes that is most likely the problem.  Unfortunately I do not have a Unicode enabled system to work with, so I have never solved these problems (although it shouldn't be too hard to do).</p>

<p>I have never used SAPRFC for PHP, but it wouldn't surprise me if they use similar functions to me, for looking up what an RFC interface looks like, so the problem will be centred around what these think an interface looks like.</p>

<p>Cheers.<br />
</p>
</div>
<p class="posted">Posted by: <a href="http://www.piersharding.com/" rel="nofollow">Piers Harding</a>  at September 16, 2005  9:34 AM</p>





<script type="text/javascript" language="javascript">
<!--
if (document.comments_form.email != undefined)
    document.comments_form.email.value = getCookie("mtcmtmail");
if (document.comments_form.author != undefined)
    document.comments_form.author.value = getCookie("mtcmtauth");
if (document.comments_form.url != undefined)
    document.comments_form.url.value = getCookie("mtcmthome");
if (getCookie("mtcmtauth") || getCookie("mtcmthome")) {
    document.comments_form.bakecookie[0].checked = true;
} else {
    document.comments_form.bakecookie[1].checked = true;
}
//-->
</script>




