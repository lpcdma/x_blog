title: "Activity生命周期"
date: 2014-03-06 21:35:41
tags:
id: 559
comment: false
categories:
  - 学习笔记
---

&nbsp;
<pre class="brush:java">public class MainActivity extends ActionBarActivity {

    private  final String TAG = "Main";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.container, new PlaceholderFragment())
                    .commit();
        }
        Log.i(TAG, "onCreate Method is executed");
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.i(TAG, "onStart Method is executed");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i(TAG, "onResume Method is executed");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        Log.i(TAG, "onRestart Method is executed");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.i(TAG, "onPause Method is executed");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i(TAG, "onStop Method is executed");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.i(TAG, "onDestroy Method is executed");
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {

        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }</pre>
[![图片1](http://lpcdma.com/wp-content/uploads/2014/03/图片1-229x300.png)](http://lpcdma.com/wp-content/uploads/2014/03/图片1.png)