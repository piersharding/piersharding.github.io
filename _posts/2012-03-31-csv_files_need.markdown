---
layout: post
title:  "CSV files need SQL"
date:   2012-03-31 12:00:00
categories: general
---


<p>As part of learning about <a href="http://www.r-project.org/">R</a> it soon has become apparent that the basic unit of currency is a CSV file - there are lots of other ways of getting data in and out of the R environment (JSON with library(RJSONIO), DB intefaces with library(RPostgreSQL) ...) but for the majority of work (which consists of hackery and experimentation which is why R is so attractive) CSV is the transportation mechanism.</p>
<p>
I have found that, in particular at the beginning, it is often harder to think of basic data munging concepts in R - typical tasks like sorting, grouping, data type conversion - often your language of choice (Perl, Python, or even bash) is just quicker for doing these things in the first instance when I'm paring the data down into what I want to apply some form of statistical analysis or charting too.</p>
<p>
With this in mind - I basically wanted to be able to perform SQL against a CSV file, without the hassle of loading it into a database first.
Enter a clever tool written in Haskell called <a href="http://keithsheppard.name/txt-sushi/tssql.html">txt-sushi</a>.  This enables you to do interesting things like:
</p>
<pre>
cat test.csv | tssql -table x - 'select a,b, sum(hours) AS hours_sum from x group by a,b'
</pre>
<p>
However, for my purposes tssql is too strict on handling data types, and is dependent on Haskell, so I've built my own simple CSV SQL processor - <a href="https://github.com/piersharding/csvtable">csvtable</a> in Python using <a href="www.sqlite.org">SQLite</a> as a backend.  This is surprisingly easy to do, and let's you have the benefit of the convenience and power of SQLite syntax:
</p>
<pre>
python csvtable.py \
  --where="system_code != 'LEAVE'" \
  --convert='date_epoch:date,hours:int' \
  --list="*, sum(hours) AS hours_sum, min(date_epoch) AS date_epoch_min, 
               max(date_epoch) AS date_epoch_max, count(*) AS days,
               ROUND(AVG(hours), 2) AS avg_time, MIN(hours) AS hours_min,
               MAX(hours) AS hours_max" \
  --group='organisation_code, system_code, request_id' \
  --file=test1.csv | \
 python csvtable.py --list='*, ROUND(((date_epoch_max - date_epoch_min) / (60 * 60 * 24)) + 1, 2) AS duration' > test2.csv
</pre>



<div id="a000090more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at March 31, 2012  6:46 AM</p>





