title: "android 应用层杀死进程"
date: 2014-03-31 21:02:50
tags:
id: 609
comment: false
categories:
  - 编程学习
---

<pre class="brush:java">		public void fuck(Context c) {

			ActivityManager activityManager = (ActivityManager) c.getSystemService(Context.ACTIVITY_SERVICE);
			activityManager.killBackgroundProcesses("com.example.aa");
			List&lt;RunningAppProcessInfo&gt; procInfo = activityManager
					.getRunningAppProcesses();
			for (int i = 0; i &lt; procInfo.size(); i++) {
				Log.v("proces " + i,
						procInfo.get(i).processName + " pid:"
								+ procInfo.get(i).pid + " importance: "
								+ procInfo.get(i).importance + " reason: "
								+ procInfo.get(i).importanceReasonCode);
				// First I display all processes into the log
			}
			for (int i = 0; i &lt; procInfo.size(); i++) {
				RunningAppProcessInfo process = procInfo.get(i);
				int importance = process.importance;
				int pid = process.pid;
				String name = process.processName;
				if (name.equals("manager.main")) {
					// I dont want to kill this application
					continue;
				}
				if (importance == RunningAppProcessInfo.IMPORTANCE_SERVICE) {
					Log.v("manager", "task " + name + " pid: " + pid
							+ " has importance: " + importance
							+ " WILL NOT KILL");
					continue;
				}
				Log.v("manager", "task " + name + " pid: " + pid
						+ " has importance: " + importance + " WILL KILL");
				//android.os.Process.killProcess(procInfo.get(i).pid);//首先会kill这个程序，就没法kill其他的了。
			}
			procInfo = activityManager.getRunningAppProcesses();
			for (int i = 0; i &lt; procInfo.size(); i++) {
				Log.v("proces after killings" + i,
						procInfo.get(i).processName + " pid:"
								+ procInfo.get(i).pid + " importance: "
								+ procInfo.get(i).importance + " reason: "
								+ procInfo.get(i).importanceReasonCode);
			}

		}</pre>