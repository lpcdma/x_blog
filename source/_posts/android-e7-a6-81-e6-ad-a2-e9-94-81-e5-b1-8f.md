title: "android 禁止锁屏"
date: 2015-03-23 21:32:42
tags:
id: 778
comment: false
categories:
  - 学习笔记
---

<pre class="brush:java">protected void onPause() {
		// TODO Auto-generated method stub
		super.onPause();
		if(null != mWakeLock){
			mWakeLock.release();
		}
	}

	@Override
	protected void onResume() {
		// TODO Auto-generated method stub
		super.onResume();
		pManager = ((PowerManager) getSystemService(POWER_SERVICE));
		mWakeLock = pManager.newWakeLock(PowerManager.SCREEN_BRIGHT_WAKE_LOCK
				| PowerManager.ON_AFTER_RELEASE, "");
		mWakeLock.acquire();
	}</pre>