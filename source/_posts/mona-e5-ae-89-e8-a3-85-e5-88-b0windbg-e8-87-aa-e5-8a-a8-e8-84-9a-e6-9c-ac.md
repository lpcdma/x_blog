title: "mona安装到windbg自动脚本"
date: 2013-12-12 17:17:16
tags:
id: 443
comment: false
categories:
  - 工具收集
---

<pre class="brush:py">#   Lincoln - Corelan
#   This assumes you have Windbg, Python2.7, and access to the internet.
#   Instructions:
#   1\. Open cmd.exe and run
#   2\. Once complete test the following:
#       a. Open windbg and run ".load pykd.pyd"
#       b. Run mona from windbg "!py mona"

import os,sys,urllib,zipfile,subprocess,fnmatch

def find_location(pattern,path):
	print "[+] Locating %s in %s" % (pattern,path)
	for (path, dirs, files) in os.walk(path):
		if os.path.isfile(os.path.join(path,pattern)) and path.lower().find("x64") == -1:
			return path

def unzip_file():
	try:
		print "[+] Unzipping download"
		zip = zipfile.ZipFile(r"pykd.zip")
		zip.extractall("unzipped_pykd")
		zip.close()
	except:
		print "[-] Error Unzipping"            

def download_files():
	try:
		print "[+] Downloading PYKD zip file from Corelan, this may take a few moments (4mb)"
		urllib.urlretrieve ("http://redmine.corelan.be/projects/windbglib/repository/raw/pykd/pykd.zip", "pykd.zip")
		unzip_file()
		print "[+] Downloading mona.py &amp; windbglib.py from Corelan"
		urllib.urlretrieve("http://redmine.corelan.be/projects/mona/repository/raw/mona.py", "unzipped_pykd\\mona.py")
		urllib.urlretrieve("http://redmine.corelan.be/projects/windbglib/repository/raw/windbglib.py", "unzipped_pykd\\windbglib.py")
	except:
		print "[-] Error Downloading"

def install_vc_file():
	try:
		print "[+] Installing VC redistributable"
		p = subprocess.Popen("unzipped_pykd\\vcredist_x86.exe").wait()
	except:
		print "[-] Error installing redistributable"

def register_dll():
	try:
		dll = find_location("msdia90.dll","C:\\")
		print ("[+] Registering 'msdia90.dll' with system located in:\n\t[*] %s" %dll)
		p = subprocess.Popen('regsvr32 "%s\\msdia90.dll"' %dll).wait()
	except:
		print "[-] Error registering DLL"

def copy_files():
	try:
		windbg = find_location("windbg.exe","C:\\")
		print ("[+] Copy pykd.pyd &amp; windbglib.py &amp; mona.py into Windbg folder:\n\t[*] %s" %windbg)
		os.system('move "unzipped_pykd\\pykd.pyd" "%s\\winext\\pykd.pyd"' %windbg)
		os.system('move "unzipped_pykd\\mona.py" "%s\\"' %windbg)
		os.system('move "unzipped_pykd\\windbglib.py" "%s\\"' %windbg)
	except:
		print "[-] Error copying file"

if __name__ == '__main__':
	try:
			download_files()
			install_vc_file()
			register_dll()
			copy_files()
			print "[+] Complete. Open Windbg and test"
	except:
			print "Error running"
			sys.exit(0)</pre>
[install_windbglib](http://lpcdma.com/wp-content/uploads/2013/12/install_windbglib.zip)

&nbsp;

&nbsp;