---
layout: post
title:  "Email, Gource, Hadoop, and Python"
date:   2012-04-12 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2012/03/csv_files_need.html">&laquo; CSV files need SQL</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

| <a href="http://www.piersharding.com/blog/archives/2012/04/hadoop_and_dumb.html">Hadoop and Dumbo &raquo;</a>

</p>

<h2>April 12, 2012</h2>

<h3>Email, Gource, Hadoop, and Python</h3>

<p>I never knew that one of the guys (Andrew C) who works at <a href="http://www.catalyst.net.nz/">Catalyst</a> wrote a fantastic times series visualisation tool called <a href="http://code.google.com/p/gource/">Gource</a> .  It's incredible what people have done with it - just look on <a href="http://www.youtube.com/results?search_query=gource">Youtube</a>.
The focus of use seems to have been on analysis of source code repository activity, but I think there is more mileage to be had from Gource than this.  I wrote a simple Map/Reduce map chain for Hadoop (not really necessary for my volume of data) that stripped out the from/to/date information from all my mbox history since 1996.  It really is simple - all you need is to a generate a file in the customformat - eg.:
</p>
<pre>
0970518767|"DJ Adams" <DJ_Adams@rank.com>|M|Andrew_Powis/RVSUK/FES/Rank@rank.com
...
</pre>

and then pump this through Gource:<pre>
gource --start-position 0.28 --stop-position 0.29 --title 'Communication sphere since 1996' -s 1 --log-format custom email-log.txt
</pre>
<p>
You can record it as a video too:
</p>
<pre>
gource --start-position 0.28 --stop-position 0.29 --title 'Communication sphere since 1996' -s 1 \
    --log-format custom email-log.txt  -1280x720 -o - | ffmpeg -y -r 60 \
    -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -crf 1 -threads 0 -bf 0 gource-video-of-email.mp4
</pre>
And this is what it looks like:<br/>
<iframe width="560" height="315" src="http://www.youtube.com/embed/i3nag9vSdjo?rel=1&modestbranding=1" frameborder="0" allowfullscreen></iframe>

<div id="a000091more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 12, 2012  8:27 PM</p>





