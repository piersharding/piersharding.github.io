---
layout: post
title:  "Hadoop and Dumbo"
date:   2012-04-13 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2012/04/email_gource_ha.html">&laquo; Email, Gource, Hadoop, and Python</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

| <a href="http://www.piersharding.com/blog/archives/2012/04/moodle_bulk_man.html">Moodle bulk management of users, courses, and course categories &raquo;</a>

</p>

<h2>April 13, 2012</h2>

<h3>Hadoop and Dumbo</h3>

<a href="https://github.com/klbostee/dumbo">Dumbo</a> is a <a href="http://www.python.org/">Python</a> framework for writing Map Reduce flows with or without <a href="http://hadoop.apache.org/">Hadoop</a>.  It's been a pain up until now, trying to get it going as it has relied on a number of patches to Hadoop for different byte streams, type codes etc. to make it work.  No longer - as the necessary patches ave now made it into core as of <a href="http://hadoop.apache.org/common/docs/r1.0.2/releasenotes.html">1.0.2</a>.
<br/>
On Ubuntu 12.04 all I needed was the debian package from <a href="http://hadoop.apache.org/common/releases.html#Download">here</a>, (<a href="http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/">install</a> as per these instructions) and then run sudo easy_install dumbo .
<br/>
The only catch is that Dumbo does not currently recognise the Debian package layout used by the Hadoop package maintainers, so I found that I had to make a one line patch to compensate for it:
<pre>
diff --git a/dumbo/util.py b/dumbo/util.py
index a57166d..cd35df3 100644
--- a/dumbo/util.py
+++ b/dumbo/util.py
@@ -267,6 +267,7 @@ def findjar(hadoop, name):
     hadoop home directory and component base name (e.g 'streaming')"""
 
     jardir_candidates = filter(os.path.exists, [
+        os.path.join(hadoop, 'share', 'hadoop', 'contrib', name),
         os.path.join(hadoop, 'mapred', 'build', 'contrib', name),
         os.path.join(hadoop, 'build', 'contrib', name),
         os.path.join(hadoop, 'mapred', 'contrib', name, 'lib'),
</pre>
<br/>
And then run the quick tutorial example from <a href="https://github.com/klbostee/dumbo/wiki/Short-tutorial">here</a> like so:
<pre>
hadoop fs -copyFromLocal /var/log/apache2/access.log /user/hduser/access.log
hadoop fs -ls /user/hduser/
dumbo start ipcount.py -hadoop /usr -input /user/hduser/access.log -output ipcounts
dumbo cat ipcounts/part* -hadoop /usr | sort -k2,2nr | head -n 5
</pre>

<div id="a000092more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 13, 2012  5:20 PM</p>





