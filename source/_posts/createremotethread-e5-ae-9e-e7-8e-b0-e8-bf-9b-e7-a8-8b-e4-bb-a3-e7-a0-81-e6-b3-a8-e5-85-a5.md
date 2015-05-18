title: "CreateRemoteThread-实现进程代码注入"
date: 2013-10-31 10:43:46
tags:
id: 365
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">/*
大体思路:首先明确目标在NOTEPAD.EXE中弹出MessageBox.现在系统中找出NOTEPAD.EXE进程,两次用VirtualAllocEx在NOTEPAD.EXE进程声明一块可读写运行的内存块,用WriteProcessMemory写入我们代码注入的线程函数和传给线程函数的参数,最后运用CreateRemoteThread,变可让我们之前写入的线程函数运行.
技巧一:我们这里使用形如下面的形式来计算需要写入函数的大小:
#pragma check_stack (off)
static DWORD WINAPI ThreadProc (LPVOID lpParameter){}
static void AfterThreadProc (void) { }
#pragma check_stack
DWORD cbCodeSize = (BYTE*)AfterThreadProc - (BYTE*)ThreadProc; 

技巧二:我们这里声明一个HYPINJECT结构,里面存放一些函数的地址,然后传递给线程函数,不然直接在线程函数里面写Win 32 API函数,可能导致崩溃.结构如下:
typedef struct tagHYPINJECT {
       ProcLoadLibrary    fnLoad;
       ProcGetProcAddress fnGetProc;
       char MsgStr [MAX_PATH];
       char DLLName [MAX_PATH];
       char ProcName [MAX_PATH];
} HYPINJECT;
关于编写线程函数的注意事项: (自己总结加网上资料)
线程函数里面最好不要调用除Kernel32和User32以外的所有函数,貌似User32里面的部分函数在调用的时候也会出现崩溃的局面,如果要调用其他库的函数可以把LoadLibrary和GetProcAddress的地址当作线程函数的参数传过去(下面的代码就是这么实现的),不过如果已知某动态链接库已经加载了,最好还是使用GetModuleHandle取代LoadLibrary.当然如果你想调用自己写的函数的话也要把函数代码复制到目标进程里面去,然后把地址通过线程函数的参数传过去.
而且不要使用任何的静态字符串,线程函数最好写成静态(貌似也可以直接禁止 增量链接 ), 函数和变量之类大小也不要太大不然可能在VirtualAllocEx的时候就不能成功*/

#include &lt;windows.h&gt;
#include &lt;Tlhelp32.h.&gt;

typedef HINSTANCE (WINAPI *ProcLoadLibrary)(char*);
typedef FARPROC (WINAPI *ProcGetProcAddress)(HMODULE, LPCSTR);
typedef int (WINAPI *ProcMessageBox)(HWND,LPCTSTR,LPCTSTR,UINT);
typedef struct tagHYPINJECT {
       ProcLoadLibrary    fnLoad;
       ProcGetProcAddress fnGetProc;
       char MsgStr [MAX_PATH];
       char DLLName [MAX_PATH];
       char ProcName [MAX_PATH];
} HYPINJECT;
#pragma check_stack (off)
static DWORD WINAPI ThreadProc (LPVOID lpParameter)
{
       HYPINJECT* p = (HYPINJECT*)lpParameter;
       HMODULE hDLL = p-&gt;fnLoad (p-&gt;DLLName);
    ProcGetProcAddress GetProc = p-&gt;fnGetProc;
       ProcMessageBox MsgBox = (ProcMessageBox)GetProc(hDLL,p-&gt;ProcName);
    MsgBox(NULL,p-&gt;MsgStr,p-&gt;MsgStr,MB_OK);
       return 0;
}
static void AfterThreadProc (void) { }
#pragma check_stack

HYPINJECT hypInject;

BOOL InjectFunc(DWORD PID)
{
       HMODULE hk = LoadLibrary ("kernel32.dll");
       hypInject.fnLoad = (ProcLoadLibrary)GetProcAddress (hk, "LoadLibraryA");
       hypInject.fnGetProc = (ProcGetProcAddress)GetProcAddress (hk, "GetProcAddress");
       strcpy(hypInject.MsgStr," hyp's Knowledge Base");
       strcpy (hypInject.DLLName, "user32.dll");
       strcpy (hypInject.ProcName, "MessageBoxA");

       PVOID pCode = NULL;
       PVOID pData = NULL;
       BOOL bc = FALSE;
       DWORD cbCodeSize = (BYTE*)AfterThreadProc - (BYTE*)ThreadProc;

       HANDLE hProc = OpenProcess(
              PROCESS_QUERY_INFORMATION |  
              PROCESS_CREATE_THREAD     |
              PROCESS_VM_OPERATION      |
              PROCESS_VM_WRITE,           
              FALSE, PID);
       if (hProc == NULL)
       {
              return FALSE;
       }

       pCode=VirtualAllocEx(hProc,NULL,cbCodeSize,MEM_COMMIT,PAGE_EXECUTE_READWRITE);
       if(pCode == NULL)
       {
              return FALSE;
       }
       bc = WriteProcessMemory(hProc,pCode,(LPVOID)(DWORD) ThreadProc,cbCodeSize,NULL);
       if (!bc)
       {
              return FALSE;
       }

       pData = VirtualAllocEx (hProc,NULL, sizeof (hypInject), MEM_COMMIT, PAGE_EXECUTE_READWRITE);
       if(pData == NULL)
       {
              return FALSE;
       }
       bc = WriteProcessMemory (hProc, pData, &amp;hypInject, sizeof (hypInject), NULL);
       if (!bc)
       {
              return FALSE;
       }

       HANDLE ht=CreateRemoteThread(hProc,NULL,NULL,(LPTHREAD_START_ROUTINE)pCode,pData,0,NULL);
       if(ht == NULL)
       {
              return FALSE;
       }
       CloseHandle(hProc);
       return TRUE;
}

int main()
{
       HANDLE hSnapshot = NULL;
       hSnapshot=CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS,NULL);
       PROCESSENTRY32 pe;
       pe.dwSize = sizeof(PROCESSENTRY32);
       Process32First(hSnapshot,&amp;pe);
       do
       {
              if(stricmp(pe.szExeFile,"NOTEPAD.EXE")==0)
              {
                     InjectFunc(pe.th32ProcessID);
                     break;
              }
       }
       while(Process32Next(hSnapshot,&amp;pe)==TRUE);
       CloseHandle (hSnapshot);    
       return 0;
}</pre>