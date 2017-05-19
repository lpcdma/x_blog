title: "windows 7去掉桌面图标箭头批处理"
date: 2012-06-18 17:38:08
tags:
id: 72
comment: false
---

```

reg add &quot;HKEY_LOCAL_MACHINESOFTWAREMicrosoftWindowsCurrentVersionExplorerShell Icons&quot; /v 29 /d &quot;%systemroot%system32imageres.dll,196&quot; /t reg_sz /f
taskkill /f /im explorer.exe
attrib -s -r -h &quot;%userprofile%AppDataLocaliconcache.db&quot;
del &quot;%userprofile%AppDataLocaliconcache.db&quot; /f /q
start explorer


```
