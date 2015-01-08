---
layout: post
title:  "Hosting an R Repository for RSAP and RMonet"
date:   2013-02-10 12:00:00
categories: general
---


<p>I've just setup an R repository to host my R extensions that I've published.  This currently contains <a href="https://github.com/piersharding/RSAP">RSAP</a>  the SAP RFC connector, and <a href="https://github.com/piersharding/RMonet">RMonet</a>  the MonetDB connector using the Monet MAPI C API.</p>

<p>It's a very easy process as document <a href="http://cran.r-project.org/doc/manuals/R-admin.html#Setting-up-a-package-repository">here</a> .</p>

<p>This repository can be generally accessed by doing the following:<br />
setRepositories(addURLs = c(PiersHarding = "http://piersharding.com/R"))</p>

<p>Or for and individual package:<br />
install.packages('RMonet', repos=c('http://piersharding.com/R'))</p>

<div id="a000098more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at February 10, 2013  8:49 AM</p>





