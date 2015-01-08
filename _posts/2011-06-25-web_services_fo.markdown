---
layout: post
title:  "Web Services for Mahara"
date:   2011-06-25 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2010/09/moodle_oauth_an.html">&laquo; Moodle, OAuth, and Google Fusion</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

| <a href="http://www.piersharding.com/blog/archives/2012/03/journey_into_ha.html">Journey into Hadoop &raquo;</a>

</p>

<h2>June 25, 2011</h2>

<h3>Web Services for Mahara</h3>

<p><span class="mt-enclosure mt-enclosure-image" style="display: inline;"><img alt="mahara.png" src="http://www.piersharding.com/blog/mahara.png" width="160" height="160" class="mt-image-none" style="" /></span></p>

<p>As part of some work for the <a href='http://www.minedu.govt.nz'>Ministry of Education</a> for LMS -> <a href="http://myportfolio.school.nz">myPortfolio</a>  (<a href="http://www.mahara.org">Mahara</a>) integration, it became apparent that we needed a Web Services stack.  This is not particularly interesting in it's self, but it is something that an interconnected service needs, in order to participate in a Socially Networked world. <br />
Building a WS framework is not a difficult thing, but it is relatively time consuming (anything that takes more than a few weeks is considered expensive here), so the problem was, how to develop an unexciting feature that in itself does not deliver any great new user experience quickly and cheaply.  At this point it occurred to me that there might be a solution in what <a href="http://www.moodle.org">Moodle</a>  has achieved with it's <a href="http://docs.moodle.org/dev/Web_services">Web Services Framework</a> - after all, Mahara is (in a previous life) based on Moodle.</p>

<p>It turned out, that the way that Peta Skoda has developed the Moodle WSF is fundamentally based on Zend data services,  and is quite portable.  </p>

<p>To this end, I have ported it as an <a href="https://wiki.mahara.org/index.php/Plugins"> auth plugin</a> which can be <a href="https://gitorious.org/mahara-contrib/auth-webservice">downloaded here</a>, and the documentation is <a href="https://wiki.mahara.org/index.php/Plugins/Artefact/WebServices">here</a>.</p>

<p>This gives the basic features of token, and user based auth, with SOAP, XML-RPC and JSON emitting REST based services.  There are a number of other things that I'd like to add to this, the most important being OAuth based authentication, and JSON based import parameter consumption.</p>

<p>Edit: JSON based import parameter consumption, has been done, but I want to add replacing MNet to the list of things to do.<br />
</p>

<div id="a000087more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at June 25, 2011  9:12 AM</p>





