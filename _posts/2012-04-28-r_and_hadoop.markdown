---
layout: post
title:  "R and Hadoop"
date:   2012-04-28 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2012/04/moodle_bulk_man.html">&laquo; Moodle bulk management of users, courses, and course categories</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

| <a href="http://www.piersharding.com/blog/archives/2012/06/sap_with_r.html">SAP with R &raquo;</a>

</p>

<h2>April 28, 2012</h2>

<h3>R and Hadoop</h3>

<p>
<a href="http://www.r-project.org/">R</a> is my hackers language of choice for analysis work.  It really appeals to my sense of iteratively refining a solution.  To my delight, I stumbled across this set of libraries for calling out to Hadoop Mapreduce, HDFS, and HBASE directly from R - <a href="https://github.com/RevolutionAnalytics/RHadoop">RHadoop</a> .
<br/>
It was surprisingly easy to get going - especially with some patient help from <a href="https://github.com/piccolbo">Antonio</a> - the project owner.  RHadoop relies on the same fixes that <a href="https://github.com/klbostee/dumbo/wiki">Dumbo</a> requires, but the game changer here is that from <a href="http://hadoop.apache.org/common/releases.html#3+Apr%2C+2012%3A+Release+1.0.2+available">Hadoop 1.0.2</a>, all the key patches that both require are now part of core.<br/>
The thing that tripped me up was a custom .Rprofile file I was using to load, and print things at the startup for R.  This was causing R to write things to stdout which is what Hadoop streaming is using to pass data between tasks.  This corrupted the data transfer, which was killing RHadoop with a weird Java Heap error.  Anyway, once sorted out, everything runs smoothly, and I like the intuitive way things are handled in an R'esque manner. eg. take the example from the <a href="https://github.com/RevolutionAnalytics/RHadoop/wiki/Tutorial">tutorial</a> :
<pre>
> library(rmr)
Loading required package: RJSONIO
Loading required package: itertools
Loading required package: iterators
Loading required package: digest
> small.ints = to.dfs(1:10)
Warning: $HADOOP_HOME is deprecated.

12/04/28 19:17:45 INFO util.NativeCodeLoader: Loaded the native-hadoop library
12/04/28 19:17:45 INFO zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
12/04/28 19:17:45 INFO compress.CodecPool: Got brand-new compressor
> out = mapreduce(input = small.ints, map = function(k,v) keyval(v, v^2))
Warning: $HADOOP_HOME is deprecated.

packageJobJar: [/tmp/RtmpXlELmY/rmr-local-env, /tmp/RtmpXlELmY/rmr-global-env, 
                              /tmp/RtmpXlELmY/rhstr.map2cf71bf8a3a9, 
                              /home/piers/hadoop/tmp/hadoop-unjar1509588906818235502/] []
                              /tmp/streamjob912555254031649512.jar tmpDir=null
12/04/28 19:18:04 INFO mapred.FileInputFormat: Total input paths to process : 1
12/04/28 19:18:05 INFO streaming.StreamJob: getLocalDirs(): [/home/piers/hadoop/tmp/mapred/local]
12/04/28 19:18:05 INFO streaming.StreamJob: Running job: job_201204281916_0001
12/04/28 19:18:05 INFO streaming.StreamJob: To kill this job, run:
12/04/28 19:18:05 INFO streaming.StreamJob: /usr/libexec/../bin/hadoop job 
                             -Dmapred.job.tracker=192.168.1.3:9001 -kill job_201204281916_0001
12/04/28 19:18:15 INFO streaming.StreamJob: Tracking URL: http://192.168.1.3:50030/jobdetails.jsp?jobid=job_201204281916_0001
12/04/28 19:18:16 INFO streaming.StreamJob:  map 0%  reduce 0%
12/04/28 19:18:45 INFO streaming.StreamJob:  map 100%  reduce 0%
12/04/28 19:18:54 INFO streaming.StreamJob:  map 100%  reduce 17%
12/04/28 19:18:57 INFO streaming.StreamJob:  map 100%  reduce 67%
12/04/28 19:19:06 INFO streaming.StreamJob:  map 100%  reduce 100%
12/04/28 19:19:21 INFO streaming.StreamJob: Job complete: job_201204281916_0001
12/04/28 19:19:21 INFO streaming.StreamJob: Output: /tmp/RtmpXlELmY/file2cf7546b881b
> from.dfs('/tmp/RtmpXlELmY/file2cf7546b881b')
Warning: $HADOOP_HOME is deprecated.

Warning: $HADOOP_HOME is deprecated.

Warning: $HADOOP_HOME is deprecated.

[[1]]
[[1]]$key
[1] 1

[[1]]$val
[1] 1

attr(,"rmr.keyval")
[1] TRUE

[[2]]
[[2]]$key
[1] 2

[[2]]$val
[1] 4
...
</pre>
</p>


<div id="a000094more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 28, 2012  8:04 PM</p>





