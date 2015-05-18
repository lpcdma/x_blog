title: "Windows WMIC命令详解 (Windows Management Instrumentation Command-line)"
date: 2014-02-17 21:34:41
tags:
id: 506
comment: false
categories:
  - 学习笔记
---

Windows WMIC命令详解

【例】将当前系统BIOS，CPU，主板等信息输出到一个HTML网页文件，命令如下：

：：得到系统信息.bat，运行bat文件即可

::系统信息输出到HTML文件,查看帮助： wmic /?
::wmic [系统参数名] list [brief|full] /format:hform &gt;|&gt;&gt; [文件名]

wmic bios list brief /format:hform &gt; PCinfo.html
wmic baseboard list brief /format:hform &gt;&gt;PCinfo.html
wmic cpu list full /format:hform &gt;&gt;PCinfo.html
wmic os list full /format:hform &gt;&gt;PCinfo.html
wmic computersystem list brief /format:hform &gt;&gt;PCinfo.html
wmic diskdrive list full /format:hform &gt;&gt;PCinfo.html
wmic memlogical list full /format:hform &gt;&gt;PCinfo.html

PCinfo.html

WMIC命令参数帮助参考：

ALIAS - 访问本地机器上的别名

BASEBOARD - 基板 (也叫母板或系统板) 管理。

BIOS - 基本输入/输出服务 (BIOS) 管理。

BOOTCONFIG - 启动配置管理。

CDROM - CD-ROM 管理。

COMPUTERSYSTEM - 计算机系统管理。

CPU - CPU 管理。

CSPRODUCT - SMBIOS 的计算机系统产品信息。

DATAFILE - DataFile 管理。

DCOMAPP - DCOM 程序管理。

DESKTOP - 用户桌面管理。

DESKTOPMONITOR - 监视器管理。

DEVICEMEMORYADDRESS - 设备内存地址管理。

DISKDRIVE - 物理磁盘驱动器管理。

DISKQUOTA - NTFS 卷磁盘空间使用情况。

DMACHANNEL - 直接内存访问(DMA)频道管理。

ENVIRONMENT - 系统环境设置管理。

FSDIR - 文件目录系统项目管理。

GROUP - 组帐户管理。

IDECONTROLLER - IDE 控制器管理。

IRQ - 间隔请求线 (IRQ) 管理。

JOB - 提供对使用计划服务安排的工作的访问。

LOADORDER - 定义执行依存的系统服务管理。

LOGICALDISK - 本地储存设备管理。

LOGON - 登录会话。

MEMCACHE - 缓存内存管理。

MEMLOGICAL - 系统内存管理 (配置布局和内存可用性)。

MEMPHYSICAL - 计算机系统物理内存管理。

NETCLIENT - 网络客户端管理。

NETLOGIN - (某一用户的)网络登录信息管理。

NETPROTOCOL - 协议 (和其网络特点) 管理。

NETUSE - 活动网络连接管理。

NIC - 网络界面控制器 (NIC) 管理。

NICCONFIG - 网络适配器管理。

NTDOMAIN - NT 域管理。

NTEVENT - NT 事件日志的项目

NTEVENTLOG - NT 时间日志文件管理。

ONBOARDDEVICE - 母板(系统板)内置普通设适配器设备的管理。

OS - 已安装的操作系统管理。

PAGEFILE - 虚拟内存文件对调管理。

PAGEFILESET - 页面文件设置管理。

PARTITION - 物理磁盘分区区域的管理。

PORT - I/O 端口管理。

PORTCONNECTOR - 物理连接端口管理。

PRINTER - 打印机设备管理。

PRINTERCONFIG - 打印机设备配置管理。

PRINTJOB - 打印工作管理。

PROCESS - 进程管理。

PRODUCT - 安装包任务管理。

QFE - 快速故障排除。

QUOTASETTING - 设置卷的磁盘配额信息。

RECOVEROS - 当操作系统失败时，将从内存收集的信息。

REGISTRY - 计算机系统注册表管理。

SCSICONTROLLER - SCSI 控制器管理。

SERVER - 服务器信息管理。

SERVICE - 服务程序管理。

SHARE - 共享资源管理。

SOFTWAREELEMENT - 安装在系统上的软件产品元素的管理。

SOFTWAREFEATURE - SoftwareElement 的软件产品组件的管理。

SOUNDDEV - 声音设备管理。

STARTUP - 用户登录到计算机系统时自动运行命令的管理。

SYSACCOUNT - 系统帐户管理。

SYSDRIVER - 基本服务的系统驱动程序管理。

SYSTEMENCLOSURE - 物理系统封闭管理。

SYSTEMSLOT - 包括端口、插口、附件和主要连接点的物理连接点管理。

TAPEDRIVE - 磁带驱动器管理。

TEMPERATURE - 温度感应器的数据管理 (电子温度表)。

TIMEZONE - 时间区域数据管理。

UPS - 不可中断的电源供应 (UPS) 管理。

USERACCOUNT - 用户帐户管理。

VOLTAGE - 电压感应器 (电子电量计) 数据管理。

VOLUMEQUOTASETTING - 将某一磁盘卷与磁盘配额设置关联。

WMISET - WMI 服务操作参数管理。

&nbsp;

http://blog.csdn.net/kimiqiu/article/details/4806167