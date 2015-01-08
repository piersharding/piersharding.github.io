---
layout: post
title:  "RStudio Server and SSO"
date:   2014-07-20 12:00:00
categories: general
---
<p align="right">

<a href="http://www.piersharding.com/blog/archives/2013/09/integrating_r_w.html">&laquo; Integrating R with Pentaho</a> |

<a href="http://www.piersharding.com/blog/">Main</a>

</p>

<h2>July 20, 2014</h2>

<h3>RStudio Server and SSO</h3>

<a href="http://www.rstudio.com/">Rstudio Server</a> is a powerful analytics workbench that I have implemented for customers as a standalone service.  Out of the box it provides fantastic tools for code management, data access and interactive visualisations, however, it is not possible in the open source release to integrate it with Web based authentication solutions.  This is a problem for delivering application as SaaS, as most clients will come with SAML, Shibboleth, CAS or OAuth type integration requirements.<br/>
RStudio Server is a mixture of C++ and GWT, with the HTTP server side component being predominantly C++, and it turns out that it is not too hard to hack (in the old school meaning of the word).  The simplest solution for my purposes, to add in external web based authentication is to add in support for identity being passed through by headers.  it works like this:

<ul>
	<li>RStudio Server sits behind a Proxy (in this case Apache2), which is a typical implementation pattern as the proxy can handle SSL termination or integration with other domain services.</li>
	<li>The Proxy (Apache2) authenticates the user, and on success inserts a header - X-Remote-User - identifying them.</li>
	<li>RStudio Server (or any other application) then uses this header to identify the user and log them in as appropriate.</li>
</ul>
<br/>
<strong><big>Safety considerations</big></strong><br/>
If the user is identified by a header then this can obviously be injected by the client so it is imperative that:
<ul>
	<li>RStudio Server must be locked down to listen only to the Proxy service typically  by listening locally on 127.0.0.1:8787.</li>
	<li>The proxy must take care to strip out any attempt to spoof the authentication header - X-Remote-User.</li>
</ul>
<br/>
<strong><big>Authentication </big></strong><br/>
As the customer base that I'm interested in have a particular focus on SAML based authentication, I like to use <a href="https://simplesamlphp.org/">SimpleSAMLphp</a>.  This has strong support for SAML1.3, 2.0, and Shibboleth, and as an added bonus can multiplex to a wide variety of other authentication sources such as Google Apps, Yahoo, OpenID, Fb, Twitter - to name a few.<br/>
With SimpleSAMLphp comes a component called authmemcookie.  This enables SimpleSAMLphp to be setup as a Service Provider that is triggered on the HTTP 401 ErrorDocument state.  In conjunction with this, I have written a <a href="https://perl.apache.org/">mod_perl</a>  authentication handler (<a href="http://search.cpan.org/~piers/Apache-Auth-AuthMemCookie-0.04/">Apache::Auth:AuthMemCookie</a>)  that accesses the authmemcookie data, and passes the identity on in the X-Remote-User  header for the protected application - namely RStudio Server.<br/>
<br/>
<strong><big>Setting Up The Proxy - Apache2</big></strong>
<br/>
Apache2 Configuration:<br/>
<pre>
ProxyRequests Off
ErrorDocument 401 "/simplesaml/authmemcookie.php"
perlModule Apache::Auth::AuthMemCookie

    # Prompt for authentication:
    &lt;Location /rstudio&gt;
        AuthType Cookie
        AuthName "RStudio Server"
        Require valid-user
        PerlAuthenHandler Apache::Auth::AuthMemCookie::authen_handler
        PerlSetVar AuthMemCookie "AuthMemCookie"
        PerlSetVar AuthMemServers "127.0.0.1:11211, /var/sock/memcached"
        PerlSetVar AuthMemAttrsInHeaders 1
        PerlSetVar AuthMemDebug 1
    &lt;/Location&gt;
ProxyPass /rstudio http://localhost:8787
ProxyPassReverse /rstudio http://localhost:8787
</pre>
<br/>
<strong><big>
Building RStudio Server</big></strong><br/>
For my purposes, I have forked RStudio on <a href="https://github.com/piersharding/rstudio">GitHub</a>.  It would be great if this change could be up-streamed though.
<br/>
General build instructions are:<br/>
<pre>
# get the code base
git clone git@github.com:piersharding/rstudio.git
cd rstudio
# install dependencies and build for Debian
./dependencies/linux/install-dependencies-debian
# installation target directory is the same as for the RStudio packages
cmake . -DRSTUDIO_TARGET=Server -DCMAKE_BUILD_TYPE=Debug -DCMAKE_CXX_FLAGS=-I/usr/share/R/include -DCMAKE_C_FLAGS=-I/usr/share/R/inc
lude -DCMAKE_INSTALL_PREFIX=/usr/lib/rstudio-server
make
sudo make install
# now configure /etc/rstudio/rserver.conf as you would normally
</pre>
<br/>
<strong><big>
RStudio Server configuration</big></strong><br/>
The key configuration elements required in /etc/rstudio/rserver.conf are:
<pre>
# make sure that it only receives local requests from the Apache2 proxy
www-address=127.0.0.1
# enable checking of the X-Remote-User HTTP header
auth-sso-remote-user=1 
# provide a URL for redirection after RStudio Server logout 
# - this enables 3rd party signout triggering
auth-sso-signout-url=http://&lt;my host&gt;/simplesaml/module.php/core/authenticate.php?as=default-sp&logout 
</pre>





<div id="a000100more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at July 20, 2014  9:13 AM</p>





