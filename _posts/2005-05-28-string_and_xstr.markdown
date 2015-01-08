---
layout: post
title:  "STRING and XSTRING"
date:   2005-05-28 12:00:00
categories: general
---


<p>
Something I find very frustrating with ABAP (the new incarnation) is that it is so hard to do the simple things.  For example, just the other day I needed to switch some character data from internat table => XSTRING.  So how would you go about that?
</p>

<div id="a000029more"><div id="more">
<p>
What I ended up with this (unsatisfactory) solution, and I'm still thinking "surely it has to be easier than this?".
</p>
<style>
<!--
.mycode {
    border-style : solid ;
    border-width: 1px ;
    border-color: #000000 ;
    background-color: #E4E5E8 ;
 }
-->
</style>


<pre class='mycode'>
...
data:
  i_pdf        type table of tline,
  w_pdf        type tline,
  w_stringdata type string,
  w_bindata type xstring,
...
  loop at i_pdf into w_pdf.
    concatenate w_stringdata w_pdf into w_stringdata.
  endloop.

  perform convertString2XString using w_stringdata changing w_bindata.
...

data: lr_conv_ce type ref to CL_ABAP_CONV_OUT_CE.

form convertString2XString using s type string changing xs type xstring.
  data: size type i.
  if lr_conv_ce is initial.
    lr_conv_ce = CL_ABAP_CONV_OUT_CE=>CREATE( ).
  else.
    call method lr_conv_ce->RESET( ).
  endif.
  call method lr_conv_ce->WRITE
    exporting data = s
    importing len  = size.
  xs = lr_conv_ce->Get_Buffer( ).
endform.
</pre>

<p>
Now given all that - I sincerely hope that I have missed something somewhere, but I suspect that I haven't.
</p>
<p>
Now that ABAP has entered the 20th century, with moving away for a strictly fixed length storage basis (don't you love the fresh smell of COBOL in the morning ...:-), with the advent of character string, and binary data support - it has to go one major step forward, with better DWIM (Do What I Mean) conversion between native data types using the standard enables of MOVE, =, CONCATENATE, ASSIGN etc.  We shouldn't have to jump through hoops, such as above to do the ordinary dross of programming that other (arguably more modern) languages do such as Perl ($bindata = join("",@tab_of_charstrings); ).
</p>
</div></div>

<p class="posted">Posted by PiersHarding at May 28, 2005  8:23 PM</p>




<h2 id="comments">Comments</h2>

<div id="c1">
<p>How about</p>

<p>$bindata = "@tab_of_charstrings";</p>

<p>?</p>

<p>:-)</p>
</div>
<p class="posted">Posted by: <a href="http://www.pipetree.com/qmacro" rel="nofollow">DJ</a>  at May 28, 2005  9:33 PM</p>





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




