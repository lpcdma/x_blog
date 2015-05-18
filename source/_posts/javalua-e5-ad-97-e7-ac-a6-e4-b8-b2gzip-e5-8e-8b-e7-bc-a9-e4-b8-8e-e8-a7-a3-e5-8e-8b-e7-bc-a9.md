title: "java&lua字符串gzip压缩与解压缩"
date: 2014-11-07 15:45:19
tags:
id: 748
comment: false
categories:
  - 学习笔记
---

lua external:

https://github.com/brimworks/lua-zlib.git
<pre class="brush:cpp">local lz = require("zlib") 
local inflated = lz.inflate()(gzipdata)
--解压缩，</pre>
java:
<pre class="brush:java">ByteArrayInputStream compressMe = new ByteArrayInputStream(retString.getBytes());
	        ByteArrayOutputStream compressedMessage = new ByteArrayOutputStream();
	        GZIPOutputStream out = new GZIPOutputStream(compressedMessage);
	        for (int c = compressMe.read(); c != -1; c = compressMe.read()) {
	            out.write(c);
	        }
	        out.close();
	        byte buffer[] = compressedMessage.toByteArray();
	        ByteArrayInputStream bais = new ByteArrayInputStream(buffer);
	        GZIPInputStream gzis = new GZIPInputStream(bais);
	        InputStreamReader reader = new InputStreamReader(gzis);
	        BufferedReader in = new BufferedReader(reader);

	        String readed;
	        while ((readed = in.readLine()) != null) {
	            System.out.println(readed);
	        }</pre>