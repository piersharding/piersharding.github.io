---
layout: post
title:  "sapnwrfc for PHP"
date:   2009-01-27 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2007/05/sapnwrfc_for_py.html">&laquo; sapnwrfc for Python</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2009/02/moodle_integrat.html">Moodle Integration with SAP &raquo;</a>

</p>

<h2>January 27, 2009</h2>

<h3>sapnwrfc for PHP</h3>

<p>In my recent work I've been working in the Education Software area, more specifically LMSs (Learning Managment Systems).&nbsp; As my current employer is a completely Open Source oriented company (yay!), we work with <a href="http://www.moodle.org/" target="_blank">Moodle</a>.</p><p>Moodle is PHP based, so when it came to the question of integrating this with SAP, we had a stumbling block.&nbsp; The existing connector is a very good one, but it doesn't give full UNICODE support, and it doens't take advantage of the NetWeaver RFC SDK such as complex structures, and strings.</p><p>To this end I've written sapnwrfc for PHP.</p><p>You can download sapnwrfc from <a href="http://www.piersharding.com/download/php/sapnwrfc/" target="_blank">here</a>.</p><p>Follow the INSTALL instructions, and here is a bit of a taster:</p><pre>&nbsp;&lt;?php<br />dl(&quot;sapnwrfc.so&quot;);<br />echo &quot;sapnwrfc version: &quot;.sapnwrfc_version().&quot;\n&quot;;<br />echo &quot;nw rfc sdk version: &quot;.sapnwrfc_rfcversion().&quot;\n&quot;;<br />$config = array('ashost' =&gt; 'ubuntu.local.net',<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'sysnr' =&gt; &quot;01&quot;,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'client' =&gt; &quot;001&quot;,<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'user' =&gt; 'developer',<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'passwd' =&gt; 'developer',<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'lang' =&gt; 'EN',<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'trace' =&gt; '1' );<br /><br />// we must have a valid connection<br />try {<br />&nbsp;&nbsp;&nbsp; $conn = new sapnwrfc($config);<br />&nbsp;&nbsp;&nbsp; $fds = $conn-&gt;function_lookup(&quot;STFC_DEEP_STRUCTURE&quot;);<br />&nbsp;&nbsp;&nbsp; $fdt = $conn-&gt;function_lookup(&quot;STFC_DEEP_TABLE&quot;);<br />&nbsp;&nbsp;&nbsp; $parms = array('IMPORTSTRUCT' =&gt; array('I' =&gt; 123, 'C' =&gt; 'AbCdEf', 'STR' =&gt; 'The quick brown fox ...'));<br />&nbsp;&nbsp;&nbsp; $results = $fds-&gt;invoke($parms);<br />&nbsp;&nbsp;&nbsp; var_dump($results);<br />&nbsp;&nbsp;&nbsp; $parms = array('IMPORT_TAB' =&gt; array(array('I' =&gt; 123, 'C' =&gt; 'AbCdEf', 'STR' =&gt; 'The quick brown fox ...')));<br />&nbsp;&nbsp;&nbsp; $results = $fdt-&gt;invoke($parms);<br />&nbsp;&nbsp;&nbsp; var_dump($results);<br />&nbsp;&nbsp;&nbsp; $conn-&gt;close();<br />}<br />catch (Exception $e) {<br />&nbsp;&nbsp;&nbsp; echo &quot;Exception message: &quot;.$e-&gt;getMessage();<br />&nbsp;&nbsp;&nbsp; throw new Exception('Assertion failed.');<br />}<br /></pre>
<p>&nbsp;sapnwrfc is a work in progress, so testing and feedback is welcome. </p><p>A discussion has been started in the forums <a href="https://forums.sdn.sap.com/thread.jspa?threadID=1208671&amp;tstart=0" target="_blank">here</a>. </p>

<div id="a000075more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at January 27, 2009  8:49 PM</p>





