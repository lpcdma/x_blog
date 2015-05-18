title: "计算机唯一识别标识(windows)"
date: 2014-02-20 16:01:25
tags:
id: 512
comment: false
categories:
  - 学习笔记
---

C:\Users\Administrator&gt;slmgr.vbs /dti

C:\Users\Administrator&gt;slmgr.vbs /dlv

[![安装id](http://lpcdma.com/wp-content/uploads/2014/02/安装id-300x288.jpg)](http://lpcdma.com/wp-content/uploads/2014/02/安装id.jpg)

&nbsp;

根据微软官方文档的说明，安装id是根据设备信息计算的来（磁盘ID、CPUID、网卡MAC）。

在安装windows后同样也会得到一个具有全球唯一性的产品 ID。

C++获得办法：
<pre class="brush:cpp">//cl /EHsc dome.cpp
#define _WIN32_DCOM
#include &lt;iostream&gt;
using namespace std;
#include &lt;comdef.h&gt;
#include &lt;Wbemidl.h&gt;

# pragma comment(lib, "wbemuuid.lib")

int main(int argc, char **argv)
{
    HRESULT hres;

    // Step 1: --------------------------------------------------
    // Initialize COM. ------------------------------------------

    hres =  CoInitializeEx(0, COINIT_MULTITHREADED); 
    if (FAILED(hres))
    {
        cout &lt;&lt; "Failed to initialize COM library. Error code = 0x" 
            &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        return 1;                  // Program has failed.
    }

    // Step 2: --------------------------------------------------
    // Set general COM security levels --------------------------

    hres =  CoInitializeSecurity(
        NULL, 
        -1,                          // COM authentication
        NULL,                        // Authentication services
        NULL,                        // Reserved
        RPC_C_AUTHN_LEVEL_DEFAULT,   // Default authentication 
        RPC_C_IMP_LEVEL_IMPERSONATE, // Default Impersonation  
        NULL,                        // Authentication info
        EOAC_NONE,                   // Additional capabilities 
        NULL                         // Reserved
        );

    if (FAILED(hres))
    {
        cout &lt;&lt; "Failed to initialize security. Error code = 0x" 
            &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        CoUninitialize();
        return 1;                    // Program has failed.
    }

    // Step 3: ---------------------------------------------------
    // Obtain the initial locator to WMI -------------------------

    IWbemLocator *pLoc = NULL;

    hres = CoCreateInstance(
        CLSID_WbemLocator,             
        0, 
        CLSCTX_INPROC_SERVER, 
        IID_IWbemLocator, (LPVOID *) &amp;pLoc);

    if (FAILED(hres))
    {
        cout &lt;&lt; "Failed to create IWbemLocator object."
            &lt;&lt; " Err code = 0x"
            &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        CoUninitialize();
        return 1;                 // Program has failed.
    }

    // Step 4: -----------------------------------------------------
    // Connect to WMI through the IWbemLocator::ConnectServer method

    IWbemServices *pSvc = NULL;

    // Connect to the root\cimv2 namespace with
    // the current user and obtain pointer pSvc
    // to make IWbemServices calls.
    hres = pLoc-&gt;ConnectServer(
         _bstr_t(L"ROOT\\CIMV2"), // Object path of WMI namespace
         NULL,                    // User name. NULL = current user
         NULL,                    // User password. NULL = current
         0,                       // Locale. NULL indicates current
         NULL,                    // Security flags.
         0,                       // Authority (for example, Kerberos)
         0,                       // Context object 
         &amp;pSvc                    // pointer to IWbemServices proxy
         );

    if (FAILED(hres))
    {
        cout &lt;&lt; "Could not connect. Error code = 0x" 
             &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        pLoc-&gt;Release();     
        CoUninitialize();
        return 1;                // Program has failed.
    }

    cout &lt;&lt; "Connected to ROOT\\CIMV2 WMI namespace" &lt;&lt; endl;

    // Step 5: --------------------------------------------------
    // Set security levels on the proxy -------------------------

    hres = CoSetProxyBlanket(
       pSvc,                        // Indicates the proxy to set
       RPC_C_AUTHN_WINNT,           // RPC_C_AUTHN_xxx
       RPC_C_AUTHZ_NONE,            // RPC_C_AUTHZ_xxx
       NULL,                        // Server principal name 
       RPC_C_AUTHN_LEVEL_CALL,      // RPC_C_AUTHN_LEVEL_xxx 
       RPC_C_IMP_LEVEL_IMPERSONATE, // RPC_C_IMP_LEVEL_xxx
       NULL,                        // client identity
       EOAC_NONE                    // proxy capabilities 
    );

    if (FAILED(hres))
    {
        cout &lt;&lt; "Could not set proxy blanket. Error code = 0x" 
            &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        pSvc-&gt;Release();
        pLoc-&gt;Release();     
        CoUninitialize();
        return 1;               // Program has failed.
    }

    // Step 6: --------------------------------------------------
    // Use the IWbemServices pointer to make requests of WMI ----

    // For example, get the name of the operating system
    IEnumWbemClassObject* pEnumerator = NULL;
    hres = pSvc-&gt;ExecQuery(
        bstr_t("WQL"), 
        bstr_t("SELECT * FROM Win32_OperatingSystem"),
        WBEM_FLAG_FORWARD_ONLY | WBEM_FLAG_RETURN_IMMEDIATELY, 
        NULL,
        &amp;pEnumerator);

    if (FAILED(hres))
    {
        cout &lt;&lt; "Query for operating system name failed."
            &lt;&lt; " Error code = 0x" 
            &lt;&lt; hex &lt;&lt; hres &lt;&lt; endl;
        pSvc-&gt;Release();
        pLoc-&gt;Release();
        CoUninitialize();
        return 1;               // Program has failed.
    }

    // Step 7: -------------------------------------------------
    // Get the data from the query in step 6 -------------------

    IWbemClassObject *pclsObj;
    ULONG uReturn = 0;

    while (pEnumerator)
    {
        HRESULT hr = pEnumerator-&gt;Next(WBEM_INFINITE, 1, 
            &amp;pclsObj, &amp;uReturn);

        if(0 == uReturn)
        {
            break;
        }

        VARIANT vtProp;

        // Get the value of the Name property
        hr = pclsObj-&gt;Get(L"SerialNumber", 0, &amp;vtProp, 0, 0);
        wcout &lt;&lt; " OS Name : " &lt;&lt; vtProp.bstrVal &lt;&lt; endl;
        VariantClear(&amp;vtProp);

        pclsObj-&gt;Release();
    }

    // Cleanup
    // ========

    pSvc-&gt;Release();
    pLoc-&gt;Release();
    pEnumerator-&gt;Release();
    if(!pclsObj) pclsObj-&gt;Release();
    CoUninitialize();

    return 0;   // Program successfully completed.

}</pre>
至于安装id还没找到什么使用程序的方法来获得它，除了slmgr。

&nbsp;

&nbsp;