title: "cocos2dx切换到android其他activity"
date: 2014-06-04 11:28:33
tags:
id: 662
comment: false
categories:
  - 学习笔记
---

使用jni回调java代码来实现。

cocos2dx提供了JniHelper接口，../cocos2d/cocos/platform/android/jni/JniHelper.h

java activity代码
<pre class="brush:java">public class JniCallTest extends Activity {

	TextView textview;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.jnicalltest);
		textview = (TextView)this.findViewById(R.id.textView1);
		textview.setText("AAAAAA");		
	}
}</pre>
静态和非静态方法，使用意图切换到目标activity。
<pre class="brush:java">	public void cppCall_nonStatic_logsth() {
		// 非静态方法
		Log.i("cppCall_nonStatic", "test2~~~~!!!");
	}

	public static Object cppCall_logsth() {
		// 静态方法
		Log.i("cppCall", "test~~~~!!!");
		Intent intent = new Intent(AppActivity.getContext(), JniCallTest.class);
		AppActivity.getContext().startActivity(intent);
		return AppActivity.getContext();
	}</pre>
C++核心代码。
<pre class="brush:cpp">	JniMethodInfo minfo;
	jobject jobj;
	bool b = JniHelper::getStaticMethodInfo(minfo, "org/cocos2dx/cpp/AppActivity", //类路径
			"cppCall_logsth",   //静态方法名
			"()Ljava/lang/Object;");   //括号里的是参数，后面的是返回值。
	if (!b) {
		log("%s","JniHelper::getStaticMethodInfo error...");
	} else {
		jobj = minfo.env-&gt;CallStaticObjectMethod(minfo.classID, minfo.methodID);
	}

	JniHelper::getMethodInfo(minfo, "org/cocos2dx/cpp/AppActivity",
			"cppCall_nonStatic_logsth", "()V");
	if (!b) {
		log("%s","JniHelper::getMethodInfo error...");
	} else {
		log("%s","ready to invoke method...");
		minfo.env-&gt;CallVoidMethod(jobj, minfo.methodID);
	}</pre>
&nbsp;