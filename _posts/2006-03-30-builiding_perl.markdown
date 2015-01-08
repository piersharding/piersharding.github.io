---
layout: post
title:  "Builiding Perl SAP::Rfc for win32"
date:   2006-03-30 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2005/12/jabbermod_perl_2.html">&laquo; Jabber::mod_perl 0.15 released</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/04/saprfc_015_for.html">SAP::Rfc 0.15 for Ruby &raquo;</a>

</p>

<h2>March 30, 2006</h2>

<h3>Builiding Perl SAP::Rfc for win32</h3>

<p>
If you have a need to build SAP::Rfc for Perl (and this can be probably made to work for Ruby, and Python too), then this is for you.  Olivier Boudry, who has been faithfully maintaining win32 PPM build for me, has given comprehensive instructions on how-to build with the "free" Visual Studio version called VS Express 2005.
</p>
<p>
This can be found at:
<a href="http://www.piersharding.com/download/win32/Build_SAP-Rfc_with_VSE2005.pdf">How-to build SAP::Rfc on Windows using Visual Studio Express 2005</a>
</p>
<br/>
Many thanks Olivier.

<div id="a000048more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at March 30, 2006  3:51 PM</p>




<h2 id="comments">Comments</h2>

<div id="c49">
<p>Whenever I run reg.pl (in your examples ), I always get error as follows, accept() can't loop:</p>

<p>==================================================<br /><br />
**** Trace file opened at 20060412 083541 Eastern Daylig, SAP-REL 640,0,7 RFC-VER 3 643653 MT-SL<br />
<br />*> RfcAcceptExt: -a IDOCREP -g ma1uh208 -x sapgw00 -t <br />
<br />*> RfcRegisterProgram ... <br />
<br />        Server Program ID     = IDOCREP<br />
<br />        Host name of Gateway  = ma1uh208<br />
<br />        Service of Gateway    = 3300<br />
<br />        RFC-Trace             = ON<br />
<br />        SNC Own Name          = <br />
<br />        SNC Library Name      = <br />
<br />        RFC Handle            = 1<br />
<br />
<br /><br />
<br />======> CPIC-CALL: 'SAP_CMNOREGTP'</p>

<p><br />LOCATION    CPIC (TCP/IP) on local host<br />
<br />ERROR       internal error<br />
<br />            (this retcode should be handled by caller of NI-layer)</p>

<p><br />TIME        Wed Apr 12 08:35:41 2006<br />
<br />RELEASE     640<br />
<br />COMPONENT   NI (network interface)<br />
<br />VERSION     37<br />
<br />RC          -8<br />
<br />COUNTER     1</p>

<p><br />==================================================<br /><br />
I've tried different versions of SAP::Rfc, including the latest 1.41, the return is always the same. I googled the error and found no valuable search results, so that I have to beg you for help. </p>

<p>Any input is greatly appreciated.<br />
</p>
</div>
<p class="posted">Posted by: James  at April 12, 2006  1:32 PM</p>
<div id="c50">
<p>Hi James,</p>

<p>Have you checked what port your gateway is listening on?</p>

<p>Also - have you checked the entry in SM59.</p>

<p>Cheers.<br />
</p>
</div>
<p class="posted">Posted by: Piers Harding  at April 15, 2006 11:16 AM</p>





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




