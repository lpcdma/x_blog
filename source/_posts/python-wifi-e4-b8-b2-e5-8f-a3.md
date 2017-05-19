title: "python wifi串口"
date: 2012-07-07 21:56:22
tags:
id: 80
comment: false
---

最近做个东西需要串口各种串口，无线的方便点
```

import socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('192.168.1.1', 2001))
sock.send('good good !')
sock.close()

ser2net

;*******************************************************************
 ; Function:  Wifi Robot Server Software
 ; Filename:  car_server.c
 ; Author:    Jon Bennett
 ; Website:   www.jbprojects.net/projects/wifirobot
 ; Credit:    Based on TCP Server Test Example
 ;            http://pont.net/socket/index.html
 ;*******************************************************************

#include &lt;sys/types.h&gt;
 #include &lt;sys/socket.h&gt;
 #include &lt;netinet/in.h&gt;
 #include &lt;arpa/inet.h&gt;
 #include &lt;netdb.h&gt;
 #include &lt;stdio.h&gt;
 #include &lt;unistd.h&gt; 
/* close */
 #include &lt;fcntl.h&gt;
 #include &lt;string.h&gt;

#include &lt;termios.h&gt;
 #include &lt;stdio.h&gt;
 #include &lt;unistd.h&gt;
 #include &lt;fcntl.h&gt;
 #include &lt;sys/signal.h&gt;
 #include &lt;sys/types.h&gt;

#define SUCCESS 0
 #define ERROR   1

#define DEBUG 1

#define END_LINE 0x0
 #define SERVER_PORT 1500
 #define MAX_MSG 100

/* function readline */
 int read_line();

int main (int argc, char *argv[]) {

   int fd;

   int sd, newSd, cliLen;

   int result1;

   struct sockaddr_in cliAddr, servAddr;

   char line[MAX_MSG];
      fd = open(&quot;/dev/tts/1&quot;, O_WRONLY);

   if (fd &lt; 0) 
{ 
  printf(&quot;Could not open port.n&quot;); 
}

   result1 = write(fd, &quot;jbpro&quot;, 5);
   /* create socket*/

   sd = socket(AF_INET, SOCK_STREAM, 0);
    if(sd&lt;0) {
     perror(&quot;cannot open socket &quot;);
     return ERROR;
   }

   /* bind server port */

   servAddr.sin_family = AF_INET;
   servAddr.sin_addr.s_addr = htonl(INADDR_ANY);

 servAddr.sin_port = htons(SERVER_PORT);

 if(bind(sd, (struct sockaddr *) &amp;servAddr, sizeof(servAddr))&lt;0) 
{
     perror(&quot;cannot bind port &quot;);
     return ERROR;
   }

 listen(sd,5);
   int count;
 count = 0;

 while(1) {
 #if DEBUG

printf(&quot;%s: waiting for data on port TCP %un&quot;,argv[0],SERVER_PORT);
 #endif

cliLen = sizeof(cliAddr);

newSd = accept(sd, (struct sockaddr *) &amp;cliAddr, &amp;cliLen);

if(newSd&lt;0) {
       perror(&quot;cannot accept connection &quot;);
       return ERROR;
     }

    /* init line */

memset(line,0x0,MAX_MSG);
     unsigned char toWrite = 0;
     /* receive segments */

while(read_line(newSd,line)!=ERROR) {
 count++;      
 #if DEBUG

 printf(&quot;%d:  %s: received from %s:TCP%d : %sn&quot;, count, argv[0], 
      inet_ntoa(cliAddr.sin_addr),    ntohs(cliAddr.sin_port), line);
 #endif
 char *buf=&quot;0&quot;;
         //toWrite = line[0];

 toWrite = line[0] - 1; 
result1 = write(fd, &amp;toWrite, 1); 

if (result1 !=1)
 {
   printf(&quot;Error writing to serial port.n&quot;);
 }
       /* init line */

 memset(line,0x0,MAX_MSG);

     } /* while(read_line) */

  } /* while (1) */
 }

/* WARNING WARNING WARNING WARNING WARNING WARNING WARNING       */
 /* this function is experimental.. I don't know yet if it works  */
 /* correctly or not. Use Steven's readline() function to have    */
 /* something robust.                                             */
 /* WARNING WARNING WARNING WARNING WARNING WARNING WARNING       */

/* rcv_line is my function readline(). Data is read from the socket when */
 /* needed, but not byte after bytes. All the received data is read.      */
 /* This means only one call to recv(), instead of one call for           */
 /* each received byte.                                                   */
 /* You can set END_CHAR to whatever means endofline for you. (0x0A is n)*/
 /* read_lin returns the number of bytes returned in line_to_return       */
 int read_line(int newSd,char *line_to_return) {

   static int rcv_ptr=0;
   static char rcv_msg[MAX_MSG];
   static int n;
   int offset;
   offset=0;

 while(1) {
     if(rcv_ptr==0) {
       /* read data from socket */

 memset(rcv_msg,0x0,MAX_MSG); /* init buffer */

 n = recv(newSd, rcv_msg, MAX_MSG, 0); /* wait for data */

 if (n&lt;0) {
 perror(&quot; cannot receive data &quot;);
 return ERROR;
       }
 else if (n==0) {
 printf(&quot; connection closed by clientn&quot;);

 close(newSd);
 return ERROR;
       }
     }

     /* if new data read on socket */

 /* OR */
     /* if another line is still in buffer */
     /* copy line into 'line_to_return' */

while(*(rcv_msg+rcv_ptr)!=END_LINE &amp;&amp; rcv_ptr&lt;n) {
       memcpy(line_to_return+offset,rcv_msg+rcv_ptr,1);
       offset++;
       rcv_ptr++;
     }

 /* end of line + end of buffer =&gt; return line */

 if(rcv_ptr==n-1) { 
      /* set last byte to END_LINE */

 *(line_to_return+offset)=END_LINE;
       rcv_ptr=0;
       return ++offset;
     } 

    /* end of line but still some data in buffer =&gt; return line */

if(rcv_ptr &lt;n-1) {
       /* set last byte to END_LINE */

 *(line_to_return+offset)=END_LINE;
       rcv_ptr++;
       return ++offset;
     }

/* end of buffer but line is not ended =&gt; */

 /*  wait for more data to arrive on socket */

 if(rcv_ptr == n) {
       rcv_ptr = 0;
     } 

  } /* while */
 }


```
