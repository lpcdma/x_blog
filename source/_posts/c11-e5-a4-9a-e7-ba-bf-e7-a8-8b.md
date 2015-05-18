title: "c++11 多线程"
date: 2014-06-18 12:17:45
tags:
id: 678
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">//.h
void myThreadA(); //定义子线程函数A
void myThreadB();//定义子线程函数B
int tickets;//计数
std::mutex mutex;////线程互斥对象，用于线程同步

//.cpp
tickets = 100;

std::thread tA(&amp;HelloWorld::myThreadA,this);
std::thread tB(&amp;HelloWorld::myThreadB,this);
tA.join();
tB.join();

//函数实现。
void HelloWorld::myThreadA(){

	while(true){
		mutex.lock();
		if ( tickets &gt; 0){
			Sleep(10);
			log("A Sell %d", tickets--);
			mutex.unlock();
		}else
		{
			mutex.unlock();
			break;
		}
	}
}

void HelloWorld::myThreadB(){

	while(true){
		mutex.lock();
		if ( tickets &gt; 0){
			Sleep(10);
			log("B Sell %d", tickets--);
			mutex.unlock();
		}else
		{
			mutex.unlock();
			break;
		}
	}
}

</pre>