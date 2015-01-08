---
layout: post
title:  "IM for SAP with Jabber (XMPP)"
date:   2006-10-06 12:00:00
categories: general
---


<div id="container">

<div id="banner">
<h1><a href="http://www.piersharding.com/blog/" accesskey="1">Where on Earth is Piers?</a></h1>
<h2></h2>
</div>

<div class="content">

<p align="right">
<a href="http://www.piersharding.com/blog/archives/2006/09/saprfc_ruby_on.html">&laquo; saprfc Ruby on Rails with AJAX</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2006/10/sap_and_rails_i.html">SAP the Enterprise, and Rails interest is building &raquo;</a>

</p>

<h2>October  6, 2006</h2>

<h3>IM for SAP with Jabber (XMPP)</h3>

<p>
<a href="http://www.jabber.org" target="_blank">Jabber and IM</a> is very much in the ascendancy at the moment, thanks to <a href="http://www.google.com/talk" target="_blank">Google Talk</a> which uses the very same <a href="http://www.xmpp.org" target="_blank">XMPP</a> protocol for messaging.</p>
<p>
With this in mind I would like to demonstrate how to easily integrate at least some minimal IM capability into SAP, being able to pass basic messages back and forth between IM accounts and R/3 accounts, and then to touch on the potential for IM beyond this.
</p>
<h3> A bit of background</h3>
<p>
<h4>XMPP</h4>
Is an XML streaming protocols for instant messaging and presence developed within the Jabber community. Because the transmission of data is encapsulated in XML, and must conform to the controlling rules of XML, coupled with implementation rules for the protocol such as dialback for server to server communication etc., making XMPP a very secure messaging platform.  Testimony to this is the absence of SPAM, in fact it could be robustly argued that if the backbone of SMTP was replaced with XMPP then SPAM would be history (but that is for another day).<br>
Additionally, XMPP is a recognised Standard - the Internet Engineering Task Force (IETF) has formalized the core XML streaming protocols as an approved instant messaging and presence technology under the name of XMPP, and the XMPP specifications have been published as RFC 3920 and RFC 3921.
</p>
<p>
<h4>Jabber</h4>
being the historical root of XMPP, has a number of server, client and component implementations surrounding it.  In brief, a Jabber environment requires a:
<ul>
<li> Server, which minimally consists of a core router or hub that directs XML streams between end points - usually those endpoints are components.</li>
<li>
Components: implement either server type features, or provide gateways or "transports" between the XMPP protocol and what ever else (in this case SMTP).  Server type features may include offline storage for messages, Client to server connection handling, authentication, Server to Server communication etc.
</li>
<li>
Clients - typically what the enduser experiences as a chat interface, but could equally be an automaton that is a message producer/consumer such as an SAP-XI node, MQ-Series  etc.
</li>
</ul>
For a more indepth ovreview, then <a href="http://www.jabber.org/about/techover.shtml" target="_blank">this</a> from the JSF is a good starting point.
</p>
<h3>Jabber and SAP</h3>

<p>
For my trial implementation, I didn't want to change an R/3 system in anyway, so the logical solution is to start with what SAP has to offer by way of a messaging interface, namely the BCS (Business Communication Service).  It is concievably quite easy to modify the BCS to include a new service class along side SMTP, SMS, Faxing etc., but inorder to keep this as "zero modification", I have decided against that route.
</p>
<p>
The basic solution design is to use email on the R/3 side that is to be bi-directionally translated to Jabber based IM via a custom built XMPP transport component (SMTP Component pictured below).
</p>
<p>
<img src="http://www.piersharding.com/download//im.jpg" width="406" height="388" border="0" alt="image" />
</p>
<p>
Flow of messages between R/3 and Jabber Server.
</p>
<h4>Message Flow and Addresses</h4>
<p>
Because the messages are moving between two protocols, it is  necessary to translate the from and to addresses between them, as the messages move through the SMTP transport Component.  This is so that a message can find its correct XMPP destination (R/3 -> Jabber), and that the IM Message can be stamped with the appropriate From address so that when a reply is made, it will be able to find it's way back again (Jabber -> R/3).
</p>
<p>
<img src="http://www.piersharding.com/download/imflowout.jpg" width="398" height="385" border="0" alt="image" />
</p>

<h3>Configure SAPConnect for SMTP</h3>
<p>
  SMTP inbound and outbound must be configured in R/3.  There is a good discussion of this in the SAP help <a href="http://help.sap.com/saphelp_nw04/helpdata/en/af/73563c1e734f0fe10000000a114084/content.htm" target="_blank">here</a>.  In the following section I have highlighted the main parts of the configuration that I used to get my SAP Netweaver <a href="http://www.sap.com/linux" target="_blank">NW4</a> evaluation system working - depending on your release your mileage may vary.</p>
<p>
<h4>Profile Parameters</h4>
Set your profile parameters:
<pre>
   rdisp/start_icman = true
   icm/server_port_2 = PROT=SMTP,PORT=25000,TIMEOUT=180         
   is/SMTP/virt_host_0 = *:25000;                               
</pre>
</p>
<p>
<h4>SICF Virtual Host configuration</h4>
Configure your ICM via transaction SICF:
</p>
<p>
<img src="http://www.piersharding.com/download/sicf_sapconnect2.jpg" width="498" height="400" border="0" alt="image" />
<br>
Ensure that your SAPConnect Virtual Host Data is configured for SMTP.
</p>
<p>
<img src="http://www.piersharding.com/download/sicf_sapconnect.jpg" width="491" height="400" border="0" alt="image" />
<br>
On the Service Data tab, ensure that the R/3 user for email receipt is configured (including client, language etc.).
</p>
<p>
<h4>SCOT Configuration</h4>
<img src="http://www.piersharding.com/download/default_domain.jpg" width="600" height="93" border="0" alt="image" />
<br>
In transaction SCOT, select the menu option for default domain - set this as the host receiving the email.
</p>
<p>
<img src="http://www.piersharding.com/download/scot_receipt.jpg" width="576" height="361" border="0" alt="image" />
<br>
In transaction SCOT, you can optionally specify that recipt email is not expected.  This makes it tidier in terms of the monitoring view of the send process.
</p>
<p>
<img src="http://www.piersharding.com/download/smtp_node.jpg" width="574" height="400" border="0" alt="image" />
<br>
Ensure that the SMTP node is configured - it usually uses the local OS transport mechanism such as sendmail (connecting to localhost port 25), and that the address range that will be sent to has a routing table rule in place.  Make sure that the job that triggers the send process has been scheduled (SCOT => Settings => Send Jobs) - this is what shuffles the emails out on a periodic basis.
</p>
<p>
Finally - make sure that every R/3 account is configured correctly with an Internet Email Address.  This is found in SU01, under address data, and communication methods.
</p>
<h3>The Jabber Component</h3>
<p>
Now that we have R/3 configured, we can move on to the Jabber side of things.  I have created a transport component, that implements a simple gateway between XMPP, and SMTP.  The component has been developed using <a href="http://www.ruby-lang.org" target="_blank">Ruby</a> and  <a href="http://home.gna.org/xmpp4r/" target="_blank">XMPP4R</a>. By nature, a transport component has two main parts to it.  It has the portion that implements the XMPP protocol, and the second part that implements the target protocol - in this case SMTP.  There are other SMTP component implementation available  such as <a href="http://devel.amessage.info/smtp-t/" target="_blank">smtp-t</a>, but - inorder to simplify the sender and receiver addresses (See diagram above and below showing process flow and address translation) - I have decided to "roll my own".   XMPP4R is easy to use, and taps into Rubys simple threads implementation - something that is essential for developing components that have two event cycles - listiening for SMTP messages on ones side, and XMPP on the the other.
</p>
<p>
In my landscape I have a Netweaver R/3 instance with an SMTP interface named seahorse.local.net listening on 2500.  My Jabber SMTP component has an SMTP interface of sapsmtp.local.net listening on port 25, and this is registered with the XMPP router called gecko.local.net.  As both SAP, and the Component have to have both sending and receiving capabilities for SMTP, and this is all configurable, I've tried to ease this pain with a diagram:
</p>
<p>
<img src="http://www.piersharding.com/download/serverids.jpg" width="596" height="352" border="0" alt="image" />
</p>
<p>
Make sure all your designated hostnames are resolvable - this is a common mistake with Jabber.
</p>
<p>
To run the component you must have installed <a href="http://www.ruby-lang.org" target="_blank">Ruby</a> and installed the <a href="http://home.gna.org/xmpp4r/" target="_blank">XMPP4R</a> package.  This also assumes that you have access to a Jabber server - I use <a href="http://jabberd.jabberstudio.org/2/#download" target="_blank">jabberd2</a>, but there are a number available.  You can find a good list <a href="http://www.jabber.org/software/servers.shtml" target="_blank">here</a>.  <a href="http://ejabberd.jabber.ru/" target="_blank">ejabberd</a> is good universal one, especially if you need to run win32.
</p>
<p>
Once you have access to your server, and have XMPP4R installed, download the component from <a href="http://www.piersharding.com/download/ruby/smtpcomp.tgz">here</a>.       
  Unpack the .tgz file, and then cd into the smtpcomp/ directory.  Here, you need to know the addresses, and port numbers of your landscape to convert them into command line options:
</p>
<p>
<pre>
piers@gecko:~/code/ruby/smtpcomp$ ruby smtpcomponent.rb -h
        Usage: smtpcomponent.rb [-h -s -p -a -c -r -q]
        Options
          -h this help
          -s Jabber server router jid (gecko.local.net) - target Jabber server
          -p Jabber server router port (5347) - target Jabber server port for components
          -a Jabber server router auth phrase (secret) 
          -c Jabber component jid and smtpd name (sapsmtp.local.net) - name of THIS component
          -r SAP system smtpd name (seahorse.local.net) - SMTP host of SAP
          -q SAP system smtpd port (2500) - SMTP port of SAP
  
piers@gecko:~/code/ruby/smtpcomp$ 
</pre>

</p>
<p>
For my setup where I have a Jabber server, gecko.local.net, a component name of sapsmtp.local.net, and an SAP system seahorse.local.net with an SMTP service on port 2500, then the command line options operate like:
</p>
<p>
<pre>
piers@gecko:~/code/ruby/smtpcomp$ sudo ruby smtpcomponent.rb -s gecko.local.net -c sapsmtp.local.net -r seahorse.local.net -q 2500
starting component at: sapsmtp.local.net
SMTPD going to listen on: sapsmtp.local.net/25
gecko.local.net http://jabber.org/protocol/disco#info to sapsmtp.local.net

</pre>
</p>
<p>
Note: the use of sudo at the beginning of the command is to make the script run with root priveledges, as the SMTP interface of smtpcomponent.rb listens on port 25 which requires super user access.  Also - you may need to specify the port for the component to connect to the router on (-p) and a password for authenticating with the route (-a).
</p>
<h4>What happens?</h4>
<p>
To show what happens, the following transcript traces first an R/3 email -> Jabber flow, and then the reverse.  In this example, even though the sending R/3 user account is DEVELOPER, I have configured the INET mail address of the R/3 user to be piers@seahorse.local.net, in the user communication section of address details (transaction SU01).

</p>
<p>
This log section shows the SMTP transcript, the translation of addresses, and then the final construction of an IM message (in XML).
<pre>
COMM: EHLO seahorse.local.net

COMM: MAIL From:<piers@SEAHORSE.LOCAL.NET>

DID a 250 for MAIL 
COMM: RCPT To:<piers@gecko.local.net>

DID a 250 for RCPT 
COMM: DATA

Message: <message from='piers@sapsmtp.local.net' type='normal' to='piers@gecko.local.net'><subject>Is it ready yet!</subject><body>I&apos;ve been waiting all day blah blah blah ....
