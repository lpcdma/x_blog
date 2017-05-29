title: Java定长线程池
date: 2017-05-29 16:29:24
categories: 学习笔记
tags: [Java,多线程,线程池]
---

启动线程Lambda写法(java8+):
```
Runnable task = () -> { System.out.println("Task running"); };
new Thread(task).start();
```

学习记忆中java提供4种线程池,这里在是baksmali源码中遇到
记录一种FixedThreadPool,一个定长的线程池.

```
public class Pool {

    private static List<Integer> mData = null;
    private static Stack mStack = new Stack();

    static {
        mData = new ArrayList<>();
        for (int i = 10; i < 20; i++) {
            mData.add(i);
        }
        for (int i = 0; i < mData.size(); i++) {
            mStack.push(mData.get(i));
        }
    }

    public synchronized static int DataPop(int index){
        //int value = data.get(index);
        return (int)mStack.pop();
        //return value;
    }

    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(10);
        List<Future<Boolean>> tasks = new ArrayList<>();
        for (int i = 0; i < mData.size(); i++) {
            final int tmpI = i;
            tasks.add(executor.submit(new Callable<Boolean>() {
                @Override
                public Boolean call() throws Exception {
                    Thread.sleep(10000);
                    System.out.println(DataPop(tmpI));
                    return Boolean.TRUE;
                }
            }));
        }
        executor.shutdown();
    }
}
```