title: "python后台&目录扫描"
date: 2012-06-12 18:05:26
tags:
id: 6
comment: false
---

[code lang="py"]
import httplib
import socket
import sys
from optparse import OptionParser

var1 = 0
var2 = 0

parser = OptionParser(usage=&quot;&quot;&quot;
fuck! very easy!!!
python fuck.py -f php.txt -u http://www.fuck.com
&quot;&quot;&quot;)
parser.add_option('-f','--file',dest='filename',
                  help=&quot;read directory from FILENAME&quot;)
parser.add_option('-u','--url',dest='url',
                  help=&quot;website url&quot;)
opts, args = parser.parse_args()
if not opts.filename:
    parser.print_help()
    sys.exit(1)
filename = opts.filename
f = open(filename)
lines = f.readlines()
f.close()
try:
    site = opts.url
    site = site.replace(&quot;http://&quot;,&quot;&quot;)
    print (&quot;tChecking website &quot; + site + &quot;......&quot;)
    conn = httplib.HTTPConnection(site)
    conn.connect()
    print &quot;t[$] Yes...... Server is Online.&quot;
except (httplib.HTTPResponse, socket.error) as Exit:
    raw_input(&quot;t [!] Server offline or invalid URL&quot;)
    exit()

print(&quot;t [+] Scanning &quot; + site + &quot;......nn&quot;)
for admin in lines:
    admin = admin.replace(&quot;n&quot;,&quot;&quot;)
    admin = &quot;/&quot; + admin
    host = site + admin
    print (&quot;t [#] Checking &quot; + host + &quot;......&quot;)
    connection = httplib.HTTPConnection(site)
    connection.request(&quot;GET&quot;,admin)
    response = connection.getresponse()
    var2 = var2 + 1
    if response.status == 200:
        var1 = var1 + 1
        print &quot;%s %s&quot; % ( &quot;nn&gt;&gt;&gt;&quot; + host, &quot;Admin page found!&quot;)
        raw_input(&quot;Press enter to continue scanning.n&quot;)
    elif response.status == 404:
        var2 = var2
    elif response.status == 302:
        print &quot;%s %s&quot; % (&quot;n&gt;&gt;&gt;&quot; + host, &quot;Possible admin page (302 - Redirect)&quot;)
    else:
        print &quot;%s %s %s&quot; % (host, &quot; Interesting response:&quot;, response.status)
    connection.close()
print(&quot;nnCompleted n&quot;)
print var1, &quot; Admin pages found&quot;
print var2, &quot; total pages scanned&quot;
raw_input(&quot;[/] The Game Over; Press Enter to Exit&quot;)

[/code]