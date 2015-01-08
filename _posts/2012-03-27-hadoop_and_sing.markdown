---
layout: post
title:  "Hadoop and single file to mapper processing flow"
date:   2012-03-27 12:00:00
categories: general
---


<p>It seems like a trivial thing to want to do, but it appears that the standard Hadoop workflow is to treat all input files as line oriented transactions, which does not help at all when I want to process on a file by file basis.  The example I was working through is where I have 20 years worth of mbox email files.  Each file needs to be broken into individual emails, the contents parsed, and useful information in the headers stripped out into a convenient format for subsequent processing.  To do this in the context of Hadoop is slightly odd.  It appears that the usual approach is to create an input file of mbox file names (loaded into HDFS), and then each mapper execution uses the HDFS API to pull the file and process it.</p>

<p>This presented another problem - in Python, how do you access the HDFS API?  There are two existing integrations that  I can find - https://github.com/traviscrawford/python-hdfs, and http://code.google.com/p/libpyhdfs/.  <a href="https://github.com/traviscrawford">Travis Crawfords'</a> is easy to get going, but as it uses a JNI binding I didn't relish the prospect of trying to make sure CLASSPATHs etc are right across all my Hadoop nodes (which for my purposes are any machine that I can beg, borrow or steal), in light of this I created my own cheap and cheerful library  that uses subprocess to call the 'hadoop' executable for 'fs' - <a href="https://github.com/piersharding/hdfsio">hdfsio</a> .<br />
I admit this isn't the height of efficiency (or possibly elegance), but it is surprisingly robust and very simple.<br />
</p>

<div id="a000089more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at March 27, 2012  6:13 AM</p>





