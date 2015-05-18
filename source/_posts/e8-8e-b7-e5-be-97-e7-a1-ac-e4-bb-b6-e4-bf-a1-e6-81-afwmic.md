title: "获得硬件信息WMI(C)"
date: 2014-02-21 00:15:25
tags:
id: 518
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">#include &lt;stdio.h&gt;
#include &lt;tchar.h&gt;
#include &lt;windows.h&gt;
#include &lt;wbemidl.h&gt;

#pragma comment(lib, "wbemuuid.lib")

void getInfo(TCHAR tmp3[100],TCHAR tmp[100], TCHAR tmp2[100]);
void _tmain(int argc, TCHAR* argv[])
{
	getInfo(TEXT("CPU NAME"),TEXT("Processor"),TEXT("Name"));
	getInfo(TEXT("CPU ID"), TEXT("Processor"), TEXT("ProcessorId"));
	getInfo(TEXT("MAC ADDRESS"), TEXT("NetworkAdapter"), TEXT("MACAddress"));
	getInfo(TEXT("DISK PNPDeviceID"), TEXT("DiskDrive"), TEXT("PNPDeviceID"));
	getInfo(TEXT("SerialNumber"), TEXT("OperatingSystem"), TEXT("SerialNumber"));
}

void getInfo(TCHAR tmp3[100],TCHAR tmp[100],TCHAR tmp2[100])
{
	// result code from COM calls
	HRESULT hr = 0;
	TCHAR tmp1[100];
	swprintf(tmp1, 100, TEXT("SELECT * FROM Win32_%s"),tmp);
	// COM interface pointers
	IWbemLocator         *locator = NULL;
	IWbemServices        *services = NULL;
	IEnumWbemClassObject *results = NULL;

	// BSTR strings we'll use (http://msdn.microsoft.com/en-us/library/ms221069.aspx)
	BSTR resource = SysAllocString(TEXT("ROOT\\CIMV2"));
	BSTR language = SysAllocString(TEXT("WQL"));
	BSTR query = SysAllocString(tmp1);//TEXT("SELECT * FROM Win32_Processor"));

	// initialize COM
	hr = CoInitializeEx(0, COINIT_MULTITHREADED);
	hr = CoInitializeSecurity(NULL, -1, NULL, NULL, RPC_C_AUTHN_LEVEL_DEFAULT, RPC_C_IMP_LEVEL_IMPERSONATE, NULL, EOAC_NONE, NULL);

	// connect to WMI
	hr = CoCreateInstance(&amp;CLSID_WbemLocator, 0, CLSCTX_INPROC_SERVER, &amp;IID_IWbemLocator, (LPVOID *)&amp;locator);
	hr = locator-&gt;lpVtbl-&gt;ConnectServer(locator, resource, NULL, NULL, NULL, 0, NULL, NULL, &amp;services);

	// issue a WMI query
	hr = services-&gt;lpVtbl-&gt;ExecQuery(services, language, query, WBEM_FLAG_BIDIRECTIONAL, NULL, &amp;results);

	// list the query results
	if (results != NULL) {
		IWbemClassObject *result = NULL;
		ULONG returnedCount = 0;

		// enumerate the retrieved objects
		while ((hr = results-&gt;lpVtbl-&gt;Next(results, WBEM_INFINITE, 1, &amp;result, &amp;returnedCount)) == S_OK) {
			VARIANT name;

			// obtain the desired properties of the next result and print them out
			hr = result-&gt;lpVtbl-&gt;Get(result, tmp2, 0, &amp;name, 0, 0);
			if (name.bstrVal != NULL)
				wprintf(TEXT("%s:%s\r\n"),tmp3, name.bstrVal);

			// release the current result object
			result-&gt;lpVtbl-&gt;Release(result);
		}
	}

	// release WMI COM interfaces
	results-&gt;lpVtbl-&gt;Release(results);
	services-&gt;lpVtbl-&gt;Release(services);
	locator-&gt;lpVtbl-&gt;Release(locator);

	// unwind everything else we've allocated
	CoUninitialize();

	SysFreeString(query);
	SysFreeString(language);
	SysFreeString(resource);
}</pre>