title: Android简单反调试
date: 2015-07-26 01:50:24
tags:
categories:
  - 学习笔记
---

##原理
开一个线程去检测/proc/$pid/status中的TracerPid

- 未调试状态TracerPid ＝ 0
- 调试状态TracerPid ＝ $gdbserver_pid


```cpp
void be_attached_check()
{
	try
	{
		const int bufsize = 1024;
		char filename[bufsize];
		char line[bufsize];
		int pid = getpid();
		sprintf(filename, "/proc/%d/status", pid);
		FILE* fd = fopen(filename, "r");
		if (fd != nullptr)
		{
			while (fgets(line, bufsize, fd))
			{
				if (strncmp(line, "TracerPid", 9) == 0)
				{
					int statue = atoi(&line[10]);
					LOGD("%s", line);
					if (statue != 0)
					{
						LOGD("be attached !! kill %d", pid);
						fclose(fd);
						int ret = kill(pid, SIGKILL);
					}
					break;
				}
			}
			fclose(fd);
		} else
		{
			LOGD("open %s fail...", filename);
		}
	} catch (...)
	{

	}

}
void thread_task(int n)
{
	while (true)
	{
		LOGD("start be_attached_check...");
		be_attached_check();
		std::this_thread::sleep_for(std::chrono::seconds(n));
	}
}
void anti_debug()
{
	LOGD("call anti_debug ......");
	auto checkThread = std::thread(thread_task, 1);
	checkThread.detach();
}
```