---
layout: post
title:  "Integrating R with Pentaho"
date:   2013-09-10 12:00:00
categories: general
---


<p><strong><a href="https://github.com/piersharding/RPentaho">RPentaho</a> - R integration for Pentaho based on community tools.</strong></p>

<p>Recently,  I've been involved in a project that has implemented <a href="http://community.pentaho.com/">Pentaho</a>  for an Analytics solution for <a href="https://moodle.org/">Moodle</a> .  This is a large (and probably will be very large) Moodle implementation, so standard Moodle reporting is just not up to it. <br />
One of the requirements was to be able to export student interaction and activity completion data.  This can quickly become huge, and the standard Pentaho CSV exporting interfaces can't cope, but there is a good solution to this based on the WebDetails <a href="http://www.webdetails.pt/ctools/cda.html">CDA</a> and <a href="http://www.webdetails.pt/ctools/cdb.html">CDB</a> work.  What WebDetails have done, is provide an excellent authenticated JSON API for common Pentaho queries, whether they be Saiku Analytics or Saiku Adhoc queries.  With this, a user can use the familiar tools to design a query, and then bookmark it.  </p>

<p>To complete the loop, I've written a library for <a href="http://www.r-project.org/">R</a> that uses the JSON interface to access these stored queries, and import the data as a standard data.frame object - <a href="https://github.com/piersharding/RPentaho">RPentaho</a>.<br />
</p>

<div id="a000099more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September 10, 2013  9:02 AM</p>





