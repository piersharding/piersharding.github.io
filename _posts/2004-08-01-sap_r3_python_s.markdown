---
layout: post
title:  "SAP R/3 + Python = saprfc"
date:   2004-08-01 12:00:00
categories: general
---


<p>
saprfc for Python has been updated, to include support for registered RFCs
(SAP R/3 ABAP -> Python), and for Transactional RFC (queued RFC).
</p>
So for example you can do this:
<pre>
class reg:

        def __init__(self):
                self.cnt = 0
                import sys
                import os

        def LoopHandler(self, srfc):
                self.cnt += 1
                print "inside the customer loop handler - iter: %d ...\n" % self.cnt
                if self.cnt >= 30:
                        return -1
                return 1

        def handler(self, iface, srfc):
                data = []
                out = os.popen(iface.COMMAND.getValue(), "r")
                for row in out:
                        data.append(row)
                out.close()
                iface.PIPEDATA.setValue( data )
                return 1

if __name__ == "__main__":
        import saprfc

        conn = saprfc.conn(gwhost='seahorse.local.net', gwserv='3300', tpname='wibble.rfcexec')

        cb = reg()

        ifc = saprfc.iface("RFC_REMOTE_PIPE", callback=cb)
        ifc.addParm( saprfc.parm("COMMAND", "", saprfc.RFCIMPORT, saprfc.RFCTYPE_CHAR, 256))
        ifc.addParm( saprfc.parm("READ", "", saprfc.RFCIMPORT, saprfc.RFCTYPE_CHAR, 1))
        ifc.addParm( saprfc.tab("PIPEDATA", "", 80))

        conn.iface(ifc)

        conn.accept(callback=cb, wait=3)
        print "All finished (reg.py) \n"
</pre>
<br/>
The latest version can be found <a
href='http://www.piersharding.com/download/saprfc-0.05.tar.gz'>here (saprfc-0.05)</a>.

<div id="a000016more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at August  1, 2004 12:50 AM</p>





