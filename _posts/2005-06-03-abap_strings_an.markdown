---
layout: post
title:  "ABAP, Strings and Concatenation"
date:   2005-06-03 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2005/06/saprfc_136_for_1.html">&laquo; SAP::Rfc 1.36 for Active State Perl 5.8.x</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2005/06/ruby_on_rails_a.html">Ruby on Rails and ActiveRecord &raquo;</a>

</p>

<h2>June  3, 2005</h2>

<h3>ABAP, Strings and Concatenation</h3>

Of late I have been heavily involved in writing ABAP code that seems to be coliding with UNICODE support related issues.  For example - I have a fixed-width internal table, that I want to concatenate into a string, that is the requirement of a subsequent operation such as:
<pre class='code'>
  CALL METHOD CL_HTTP_UTILITY=>ENCODE_BASE64
    EXPORTING
      UNENCODED = w_stringdata
    RECEIVING
      ENCODED   = base64data.
</pre>

<div id="a000031more"><div id="more">
In this instance, you would expect to beable to just loop through a table, and concatenate the lengths of fixed working storage into a single string like so:
<pre class='code'>
  loop at i_pdf into w_pdf.
    concatenate w_stringdata w_pdf into w_stringdata.
  endloop.
</pre>
But this does not appear to be the case - try this:
<pre class='code'>
data:
  begin of rec,
    a(5),
  end of rec,
  w_row like rec,
  i_rows like standard table of rec,
  w_string type string.
  do 10 times.
    write sy-index to w_row right-justified.
    append w_row to i_rows.
  enddo.
  loop at i_rows into w_row.
    concatenate w_string w_row into w_string.
  endloop.
</pre>
<p>
w_string will not be the expected 50 characters long, as it appears that the concatenate has removed a trailing space from each iteration of i_rows.
</p>
<p>
Basically - whats happening is that ABAP does not honour the true value of the fixed length character working storage.  To me - fixed length is fixed length - if I wanted less than what was exactly in the piece of working storage, then I would expect to explicitly say so, not have something silent syphon it away.  What is happening doesn't even conform to the generally accepted rules of C, where a string is NULL terminated, not the last non-whitespace value.
</p>
<p>
So - how do I get around this?  So far, I have only found two, fairly rubbish solutions to this.
<br/>
One is to pre-stretch a string, and then to specifically replace chunks of it with the value that I really want:
<pre class='code'>
* operation pre-stretch - assume that fixed length ws is 5 chars
* must do this 5+1
  describe table i_row lines w_rows.
* mark beginning and end of string
  w_blank+0(1) = 'x'.
  w_blank+5(1) = 'x'.
  do w_rows times.
    concatenate w_stringdata w_blank into w_stringdata.
  enddo.

* now replace the sections of the string with the real data
  loop at i_rows into w_row.
    REPLACE SECTION offset w_cnt LENGTH 5 OF w_stringdata WITH w_row.
    w_cnt = w_cnt + 5.
  endloop.
</pre>

or - I did this:
<pre class='code'>
data:
* see other data declarations from above
  astring type string,
  fname(100) type c value '/tmp/shimmy.txt'.

  open dataset fname for output in binary mode.
  loop at i_rows into w_row.
    transfer w_row to fname.
  endloop.
  close dataset fname.

  open dataset fname for input in binary mode.
  read dataset fname into astring.
  close dataset fname.
</pre>
</p>
<p>
This was trialed on NW4 test drive edition, and Enterprise 4.7. <br/>
I'd be really interested to hear of any other solutions that people have - or someone pointing out a glaringly obvious mistake that I've made?  And for extra points - figure out why I had to pre-stretch the string with a size greter than the sum total of parts.
</p>
</div></div>

<p class="posted">Posted by PiersHarding at June  3, 2005 10:06 AM</p>




<h2 id="comments">Comments</h2>

<div id="c11">
<p>I download a file which has a string of 10 size but<br />
the data is only 5 chars.<br />
In the downloaded text file  the cursor position ends at 5 i want the cursor position shall end at 10.<br />
akshay.</p>
</div>
<p class="posted">Posted by: AKSHAY BIRAJDAR  at August 10, 2005  1:16 PM</p>





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




