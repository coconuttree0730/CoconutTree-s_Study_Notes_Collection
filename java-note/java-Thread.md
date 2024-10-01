# 线程-并发

## 概述

### 线程执行内存图

###### 调用： .run();

![](/home/administrator/.config/marktext/images/2024-10-01-13-14-04-image.png)

> 主线程压栈；

##### 调用: .start();

![](/home/administrator/.config/marktext/images/2024-10-01-13-15-20-image.png)

> .start() 会开启一个新的栈,在新的线程栈压入 run方法，在这个线程运行该程序...；然后主线程main就把 压入的 start方法pop出去了。

#### 线程的(创建)实现方式：

##### 1. extends Thread  (和线程有血缘关系：is thread...)

> Override : public void run(){}

- Mythread:

```java
/**
 * @author : administrator
 * @created : 2024-09-30
**/
package com.thread;
public class MyThread extends Thread{   //************
    @Override
    public void run(){ //****************8
        for(int i = 0; i < 1000 ; i++){

            System.out.println("Mythread --> " + i);
        }
    }
}
```

- MainTest:

```java
/**
 * @author : administrator
 * @created : 2024-09-30
**/
package com.thread;
public class createThreadTest{

    public static void main (String[] args) {
        Thread t = new MyThread();
        t.start();
        System.out.println("main-start...");
        for(int i = 0 ; i < 100 ; i++){
            System.out.println("main-thread ---> " + i);
        }

    }
}
```

---

##### 2. implements Runnable （<mark>更推荐</mark>，还能“多继承“其他接口）

> （和线程 无血缘关系，只是实现 Runnable的run，“能运行...”）
> 
> - 只是实现了一个接口的run方法,是一个<mark>普通的类</mark>

```java
/**
 * @author : administrator
 * @created : 2024-09-30
**/
public class createThreadTest{

    public static void main (String[] args) {

//----------------------------------------------------------    
        new Thread( new Runnable(){
            @Override
            public void run(){
                for(int i = 0 ; i < 100 ; i++){
                    System.out.println("Runable-thread ---> " + i);
                }
            }
        } ).start(); //匿名线程：RUnnable实现...
//------------------------------------------------------------

        //main: 主线程...    
        System.out.println("main-start...");
        for(int i = 0 ; i < 100 ; i++){
            System.out.println("main-thread ---> " + i);
        }

    }
}
```

```java
/**
 * @author : administrator
 * @created : 2024-09-30
**/
package com.thread;
public class createThreadTest{

    public static void main (String[] args) {

        Thread thread_1 = new Thread(new MyRunnable());
        thread_1.start(); 
        //thread_1.getName();
        //thread_1.currentThread();

        System.out.println("main-start...");
        for(int i = 0 ; i < 10 ; i++){
            System.out.println("main-thread ---> " + i);
        }

    }
}



//MyRunnable: ==============================================

package com.thread;
public class MyRunnable implements Runnable{
    @Override
    public void run(){
        for(int i = 0; i < 10 ; i++){

            System.out.println("Mythread --> " + i);
        }
    }
}
```

##### 线程方法：

###### 1. getName(); 获取线程名字；  - 实例方法

###### 2.setName(String name) ；修改线程名；  - 实例方法

```java
Thread thread = new Thread();
thread.setName("setNewThreadName");

//or 
Thread tttt = new Thread("newThreadName ...")
tttt.getName(); // "newThreadName ..."

//or  ===============================================================>    
Thread thread_2 = new MyThread("newThreadName");

public class MyThread extends Thread{

    public MyThread(String threadName){
        super(threadName);    //继承父类的字段...
    }

    @Override
    public void run(){
        for(int i = 0; i < 1000 ; i++){

            System.out.println("Mythread --> " + i);
        }
    }
}
```

###### 3. currentThread() ；  获取线程对象引用...  - 静态方法

> static **Thread** currentThread(); 

```java
Thread t = new Thread();
t.getName(); 
t.setName("modify-threadName");
Thread temp = t.currentThread(); //返回一个引用指向 ：一个线程对象

String threadName = new Thread().currentThread().getName(); //获取当前线程的名字..
```

```java
.setName();
t.setName("settingMain"); 线程名： main ---> settingMan
```

----

### 线程对象生命周期

#### 线程状态：(6种，对应着线程生命周期)

Thread.State

    ![](/home/administrator/.config/marktext/images/2024-10-01-14-21-52-image.png)

- New ：创建

- Runnable : 可运行： 就绪（就绪队列） and 运行

- Blocked ： 阻塞 ： 遇到锁Lock （线程同步-加锁）就进入阻塞

- Waiting : 等待 （等待队列）： 无限期的等待cpu

- Timed_Waiting ：(超时等待) ：有等待时长限制，如：sleep(1000) : 等待100ms后唤醒

> .notify : 是“唤醒”的意思

- Terminated: 结束(释放)

##### 线程生命周期状态图

![](/home/administrator/.config/marktext/images/2024-10-01-14-50-49-image.png)

![](/home/administrator/.config/marktext/images/2024-10-01-15-40-56-image.png)

- 有限（非无限循环）的线程，执行完毕就销毁了，再次调用就调用不到了

-------

##### .sleep()的使用

> sleep的功能就是让<u>当前线程</u> 进行 有限时间(超时)等待...

> 1 s = 1000ms
> 
> - 线程使用 .sleep之后，会释放cpu执行，转而进入堵塞态(等待状态...wait多久，根据传入sleep的参数时间决定)
> 
> - Thread.sleep(1000);//将当前位置的线程进行休眠1秒
> 
> - .sleep();有异常抛出...<mark>InterruptedException</mark>
> 
> - InterruptedException 继承于 Exception: 是编译检查异常...需要尝试捕获...

- static void sleep(毫秒,纳秒);

- static void sleep(毫秒)；

- ... ...

```java
//sleep() 实例：
public class ThreadSleepTest{

    public static void main(String[] args){
        System.out.println("main-start...");
        System.out.println("sleep 3 s ...");
        try{
           Thread.sleep(3000);//堵塞3秒
        }catch(InterruptedException e){

            System.out.println(e.getMessage());

        }
        System.out.println(".... dang !");

    }

}



//----------------------
public class PrintDot{
    public static void main(String[] args){
        try{
            while(true){
                Thread.sleep(1000);
                System.out.print(".");
            }
        }catch(Exception e){
            e.printStackTrace();
            System.out.println(e.getMessage());
            // throw new RuntimeException(e);//生存新的异常需要处理...不太好的感觉...
        }
    }

}
```

> .sleep的异常使用尝试捕获try-catch，<mark>不要</mark>在run方法进行 throws InterruptedException [X]

![](/home/administrator/.config/marktext/images/2024-10-01-19-59-12-image.png)

##### .interrupt();

> 终止 <mark>调用对象的</mark> 线程！：谁调用的终止谁的睡眠

```java
Thread thread = new Thread(new MyRunnable());


thread.interrupt();//sleep()是要捕获异常的，
                   // interrupt()执行，sleep()捕获到异常，就会抛出异常，实现sleep的关闭... 


```

有睡眠，想把正在睡觉中的线程提前叫醒：<mark>.interrupt( ); </mark> //实例方法...
