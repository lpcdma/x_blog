title: "linux C kill函数使用"
date: 2014-03-31 22:29:28
tags:
id: 612
comment: false
categories:
  - 编程学习
---

<pre class="brush:cpp">#include &lt;stdio.h&gt;
#include &lt;signal.h&gt;
#include &lt;sys/types.h&gt;

void main(int argc, char** argv){

kill((pid_t)atoi(argv[1]),SIGTERM);
//system("kill 12756");
}</pre>
<pre class="brush:cpp">/* ---------------------------------------------------------------- */
/* PROGRAM  process-a.c:                                            */
/*   This program demonstrates the use of the kill() system call.   */
/* This process must run before process-b.c because it creates a    */
/* shared memory segment for storing its process id.                */
/* ---------------------------------------------------------------- */

#include  &lt;stdio.h&gt;
#include  &lt;sys/types.h&gt;
#include  &lt;signal.h&gt;
#include  &lt;sys/ipc.h&gt;
#include  &lt;sys/shm.h&gt;

/* ---------------------------------------------------------------- */
/*                 signal handler function prototypes               */
/* ---------------------------------------------------------------- */

void  SIGINT_handler(int);         /* for SIGINT                    */
void  SIGQUIT_handler(int);        /* for SIGQUIT                   */

/* ---------------------------------------------------------------- */
/*                         global variable                          */
/* ---------------------------------------------------------------- */

int   ShmID;                       /* shared memory ID              */
pid_t *ShmPTR;                     /* shared memory pointer         */

/* ---------------------------------------------------------------- */
/*                   main program starts here                       */
/* ---------------------------------------------------------------- */

void main(void)
{
     int   i;
     pid_t pid = getpid();
     key_t MyKey;

     if (signal(SIGINT, SIGINT_handler) == SIG_ERR) {
          printf("SIGINT install error\n");
          exit(1);
     }
     if (signal(SIGQUIT, SIGQUIT_handler) == SIG_ERR) {
          printf("SIGQUIT install error\n");
          exit(2);
     }

     MyKey   = ftok(".", 's');     /* create a shared memory segment*/
     ShmID   = shmget(MyKey, sizeof(pid_t), IPC_CREAT | 0666);
     ShmPTR  = (pid_t *) shmat(ShmID, NULL, 0);
     *ShmPTR = pid;                /* save my pid there             */

     for (i = 0; ; i++) {          /* generating output             */
          printf("From process %d: %d\n", pid, i);
          sleep(1);
     }
}

/* ---------------------------------------------------------------- */
/* FUNCTION  SIGINT_handler:                                        */
/*    SIGINT signal handler.  It only reports that a Ctrl-C has     */
/* been received.   Nothing else.                                   */
/* ---------------------------------------------------------------- */

void  SIGINT_handler(int sig)
{
     signal(sig, SIG_IGN);
     printf("From SIGINT: just got a %d (SIGINT ^C) signal\n", sig);
     signal(sig, SIGINT_handler);
}

/* ---------------------------------------------------------------- */
/* FUNCTION  SIGQUIT_handler:                                       */
/*    SIGQUIT signal handler.   When SIGQUIT arrives, this handler  */
/* shows a message, removes the shared memory segment, and exits.   */
/* ---------------------------------------------------------------- */

void  SIGQUIT_handler(int sig)
{
     signal(sig, SIG_IGN);
     printf("From SIGQUIT: just got a %d (SIGQUIT ^\\) signal"
                          " and is about to quit\n", sig);
     shmdt(ShmPTR);
     shmctl(ShmID, IPC_RMID, NULL);

     exit(3);
}</pre>
<pre class="brush:cpp">/* ---------------------------------------------------------------- */
/* PROGRAM  process-b.c:                                            */
/*   This program demonstrates the use of the kill() system call.   */
/* This process reads in commands and sends the corresponding       */
/* to process-a.  Note that process-a must run before process-b for */
/* process-b to retrieve process-a's pid through the shared memory  */
/* segment created by process-a.                                    */
/* ---------------------------------------------------------------- */

#include  &lt;stdio.h&gt;
#include  &lt;sys/types.h&gt;
#include  &lt;signal.h&gt;
#include  &lt;sys/ipc.h&gt;
#include  &lt;sys/shm.h&gt;

void  main(void)
{
     pid_t   pid;
     key_t MyKey;
     int   ShmID;
     pid_t *ShmPTR;
     char  line[100], c;
     int   i;

     MyKey   = ftok(".", 's');          /* obtain the shared memory */
     ShmID   = shmget(MyKey, sizeof(pid_t), 0666);
     ShmPTR  = (pid_t *) shmat(ShmID, NULL, 0);
     pid     = *ShmPTR;                 /* get process-a's ID       */
     shmdt(ShmPTR);                     /* detach shared memory     */

     while (1) {                        /* get a command            */
          printf("Want to interrupt the other guy or kill it (i or k)? ");
          gets(line);
          for (i = 0; !(isalpha(line[i])); i++)
               ;
               c = line[i];
          if (c == 'i' || c == 'I') {   /* send SIGINT with kill()  */
               kill(pid, SIGINT);
               printf("Sent a SIGINT signal\n");
          }
          else if (c == 'k' || c == 'K') {
               printf("About to send a SIGQUIT signal\n");
               kill(pid, SIGQUIT);      /* send SIGQUIT with kill() */
               printf("Done.....\n");
               exit(0);
          }
          else
               printf("Wrong keypress (%c).  Try again\n", c);
     }
}</pre>
&nbsp;