---
layout: post
title:  "Write your own RFCs without using SE37!"
date:   2005-11-15 12:00:00
categories: general
---


<p>
Of late - I have found a need to be able to write RFCs on the fly - partly to be able to deliver new functionality to my user base for my RFC implementations for Perl, Python, and Ruby.  Using transports is not an option because of potential licensing issues, and the practicalities of release support.  I started looking into how you can actually write Function Modules on the fly, and ended up with SAP::Tools which I have first released for Ruby.
</p>
<p>
Here is an example (albeit a pointless one) of how to build up an interface, and then insert the code into an RFC that is ready to run.
</p>

<pre class='code'>
require "lib/SAP/Tools"

rfc = SAP::Tools.new(:ashost => "seahorse.local.net",
                   :sysnr  => 00,
                   :lang   => "EN",
                   :client => 000,
                   :user   => "DEVELOPER",
                   :passwd => "DEVELOPER",
                   :trace  => 1)

func_pool = "ZMYRFC1"
func = "ZTEST"
short_text = "Some function group - #{func_pool}"
func_desc = "description of #{func}"

# get the connection ID
print "[main] connect id: ", rfc.connection, "\n"

# test the connection
print "[main] is_connected: ", rfc.is_connected(), "\n"

if rfc.existsRFC(func)
  print "[main] deleting: #{func}\n"
  rfc.deleteRFC(func)
end

ztest = SAP::Iface.new(func)
ztest.addParm(SAP::Parm.new(:name => "PARMIMP", :type => RFCIMPORT, :like => "SY-REPID", :optional => true))
ztest.addParm(SAP::Parm.new(:name => "PARMEXP", :type => RFCEXPORT, :like => "TRDIR"))
ztest.addParm(SAP::Parm.new(:name => "SYSTEM", :type => RFCEXPORT, :like => "SY-SYSID"))
ztest.addParm(SAP::Tab.new(:name => "WAHOO", :like => "TAB512", :optional => true))

src =  [
      'TABLES: TRDIR.',
      'SELECT SINGLE * FROM TRDIR WHERE NAME = PARMIMP.',
      'SYSTEM = SY-SYSID.',
      'PARMEXP = TRDIR.',
     ]

test = rfc.makeRFC(:func => ztest,
                    :desc => func_desc,
                    :group => func_pool,
                    :grouptext => short_text,
                    :source => src)

# close the connection
print "[main] close connection: ", rfc.close(), "\n"
</pre>

<p>
You may ask why would I bother to go to this trouble, and my reply is that I
have been banging my head against SAP's inertia, and internal politics for the
last year, trying to get access to the new features for handling complex data
types in RFC . As a result I have had to build a complex framework for
automatically generating RFC to support my efforts for dynamically
instantiating ABAP Objects, and calling methods etc. from Perl, Ruby etc.
</p>
<p>
However - a side effect is that this can open up the way for accessing lots of
Function Modules that aren't RFC enabled without having to go to the trouble
of developing wrappers from within R/3 (these wrappers can be automatically
created for you).
</p>

<p>
If you think this is a "Hack" then you're right - but as I've eluded to before -
this is a Hackers (an artistic programmer as opposed to an nefarious evil-doer) response to the reluctance I have experienced from SAP, in the
pursuit of extending the sophistication of integration of R/3 with scripting
programming languages from the Open Source world.  Further to this - I am
noticing a worrying trend in Senior SAP Executive rhetoric with respect to
Open Source, and am wondering if this is signalling a change in some of the
founding principles that have carried SAP to such heights as it enjoys now.
</p>


<div id="a000043more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at November 15, 2005  8:03 AM</p>




<h2 id="comments">Comments</h2>

<div id="c33">
<p>Very interesting, this is definitely required. Keep up the good work please!</p>
</div>
<p class="posted">Posted by: Jonathan Groll  at November 17, 2005  9:47 AM</p>
<div id="c34">
<p>Really really good. I agree with your appreciation of the relation of SAP with its "open source" heritage. No doubt the openness of ABAP code has been one of the causesb of SAP success. I've been waiting very long for a seamless integration of SAP with scripting languages, but nothing is coming from the standard. Fortunately we have your work. Very well done. Keep waiting for the Perl version.</p>
</div>
<p class="posted">Posted by: Cesar  at November 24, 2005  9:43 AM</p>





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




