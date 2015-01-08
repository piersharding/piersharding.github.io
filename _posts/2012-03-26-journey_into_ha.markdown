---
layout: post
title:  "Journey into Hadoop"
date:   2012-03-26 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2011/06/web_services_fo.html">&laquo; Web Services for Mahara</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

| <a href="http://www.piersharding.com/blog/archives/2012/03/hadoop_and_sing.html">Hadoop and single file to mapper processing flow &raquo;</a>

</p>

<h2>March 26, 2012</h2>

<h3>Journey into Hadoop</h3>

<p>I've been building up my background knowledge on current toolsets used in Data Science, and part of this is <a href="http://www.r-project.org/">R</a> and another is <a href="http://hadoop.apache.org/">Hadoop</a>.</p>

<p>Hadoop is a big thing, and takes (to my mind) quite a lot of effort to get going, and to understand how you can bend it to your will.  Par of this learning process has been about finding a comfortable installation pattern for Linux - in particular Ubuntu, and the best help I've found so far has been from <a href="http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/">Michael Noll</a>.  Things that I had to be careful about were getting ssh working, and name resolution exactly right on all nodes that you put in your cluster, as you distribute things like /etc/hadoop/masters and the *-site.xml config files.</p>

<p>The next stage was to find a development pattern that enabled me to avoid Java.  The answer to this for me is <a href="http://wiki.apache.org/hadoop/HadoopStreaming">Hadoop Streaming</a>.  This basically allows you to pipe IO in and out of programs written in your favourite language - and in this case Michael does brilliantly again with <a href="http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/">Python and MapReduce</a>.<br />
</p>

<div id="a000088more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at March 26, 2012  9:23 AM</p>





