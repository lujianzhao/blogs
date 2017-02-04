###守护线程
1. 必须在start方法之前；
2. 如果线程为守护线程，随着主线程结束而结束

```java
public class Test {

    public static void main(String[] args) {
        DaemonThread t = new DaemonThread();
        t.setDaemon(true);
        t.start();
        try {
            Thread.sleep(5000);

            // 监听jvm退出
            Runtime.getRuntime().addShutdownHook(new Thread() {
                @Override
                public void run() {
                    System.out.println("jvm exit");
                }
            });
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class DaemonThread extends Thread {
    @Override
    public void run() {
        // 无限循环
        for (;;) {
            try {
                System.out.println("deamon thread is runing");
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

Console打印
```java
deamon thread is runing
deamon thread is runing
deamon thread is runing
deamon thread is runing
deamon thread is runing
jvm exit
```