title: "php简单二进制加密"
date: 2014-08-15 14:50:24
tags:
id: 708
comment: false
categories:
  - 学习笔记
---

<pre class="brush:php">/*通过简单异或对文件加密，
switch 可以去掉。
*/
&lt;?php
function encode($readFile,$writeFile){
$fread = fopen($readFile,"rb");
$file = fopen($writeFile, "wb");
if($fread){
	while (!feof($fread)){
		$bin = fread($fread,4);
		//print gettype($bin);
		switch (strlen($bin))
		{
		case 1:
			$bin = $bin ^ "\x41";
			break;
		case 2:
			$bin = $bin ^ "\x41\x41";
			break;
		case 3:
			$bin = $bin ^ "\x41\x41\x41";
			break;
		case 4:
			$bin = $bin ^ "\x41\x41\x41\x41";
			break;
		default:
			break;
		}
		fwrite($file,$bin);//unpack("H*", $bin));
	}
}
fclose($file);
fclose($fread);
}
encode("/Users/****/Downloads/fuck.gz.bak","/Users/****/Downloads/fuck.gz.bak.gz");
?&gt;</pre>