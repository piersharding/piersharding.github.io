---
layout: post
title:  "Moodle, OAuth, and Google Fusion"
date:   2010-09-06 12:00:00
categories: general
---


<p>Convergence is a strange and reoccurring theme, and it's happened again from me over the last few months with BI reporting, <a href="http://www.moodle.org">Moodle</a>, <a href="http://oauth.net">OAuth</a>, and Google.</p>

<p>I've looked at a few BI (well <a href="http://www.sdn.sap.com/irj/sdn/edw">SAP</a>, <a href="http://en.wikipedia.org/wiki/Crystal_Reports">Business Objects</a>, and <a href="http://www.pentaho.com/">Pentaho</a>) implementations over the years, and one of the things that I have always found frustrating/off putting is what I consider the huge startup costs for such implementations.  This has usually been characterised by expensive infrastructure implementations in both hardware and software coupled with the difficulty that most businesses have in visualising what data they need to have access to, and how it should be most effectively presented.</p>

<p>I've found this dilemma more accute in the <a href="http://www.moodle.org">Moodle</a> world, as the so many of the customers involved are on a very tight to non-existent budget, yet their requirement to analyse Learning Managment System performance data is still there.</p>

<p>A year ago, I concluded that Pentaho was my first choice, for the twin reasons that it's OpenSource (specifically no license fees), and that it has sufficiently good data modelling tools to enable a suite of reports customised to Moodle to be delivered.  While this reduces the cost of delivering a flexible reporting solution for Moodle, it still falls short on a couple of points:</p>

<p>(1) Most people who implement Moodle are not Data Warehousing, or Modelling experts so they are unlikely to be able to sufficiently accurately determine what their requirements are in advance (actually a common business problem, not unique to the Moodle community).<br />
(2) Pentaho, while reasonably straight forward to install, is still another complex piece of software to host - a major barrier to entry for most Moodle implementations.</p>

<p>What I started looking for then, was a set of visualisation tools that could be integrated with Moodles PHP environment - atleast users would then be able to do more complex reporting and analysis.  What I found exceeded my expectations, in the form of a Labs project from Google called <a href="http://tables.googlelabs.com/Home">Fusion Tables</a>.</p>

<p>Fusion Tables is shaping up to be Business Intelligence reporting with the twist of collaborative, and Geo encoding capabilities.  The basic mode is that CSV files of data can be uploaded into a flexible storage engine, datasets can be joined and merged, automatically Geo encoded, and then consumed through a good set of graphical presentation tools.  <a href="http://tables.googlelabs.com/DataSource?dsrcid=197026">Datasets</a> can be shared and collaboratively edited.<br />
<script src="http://www.gmodules.com/ig/ifr?url=http://www.google.com/ig/modules/bar-chart.xml&up__table_query_url=http://tables.googlelabs.com/gvizdata?tq=select+col0%252Ccol5+from+191509++skip+0+limit+228&up__table_query_refresh_interval=0&w=600&h=400&border=%23ffffff%7C3px%2C1px+solid+%23999999&synd=open&output=js"></script></p>

<p><br />
As Luck would have it that this service is firstly free, and secondly exposed via an SQL-like <a href="http://code.google.com/apis/fusiontables/">API</a> integrated with the standard Google OAuth mechanism.  This makes it attractive as a generic data analysis and reporting tool for a low cost operating environment like Moodle and the education sector.</p>

<p>To test out the theory of all this, I've implemented 3 things:<br />
 * OAuth integration for Moodle including a site, and secret registry<br />
 * A generic Fusion Tables data proxy for Moodle<br />
 * A Gradebook export module that enables the export of the standard gradebook data to Fusion Tables</p>

<p>For the curious, this can be found at Gitorious -<a href="http://gitorious.org/moodle-local_oauth/moodle-local_oauth">moodle-local_oauth</a>.</p>

<div id="a000086more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at September  6, 2010  7:06 AM</p>





