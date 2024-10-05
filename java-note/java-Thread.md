# 线程-并发

## 概述

### 线程执行内存图

###### 调用： .run();

![](/home/administrator/.config/marktext/images/2024-10-01-13-14-04-image.png)

> 主线程压栈；

##### 调用: .start();

![](/home/administrator/.config/marktext/images/2024-10-01-13-15-20-image.png)

> .start() 会开启一个新的栈,在新的线程栈压入 run方法，在这个线程运行该程序...；然后主线程main就把 压入的 start方法pop出去了。

#### 线程的(创建)实现方式：4种

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

##### 3.Callable : 线程可返回值

> 可通过 xxx.get();获取线程返回值;
> 
> .get(); 和 join(  ) 很像，会堵塞主线程，等它执行完毕才唤醒主线程继续往下执行

```java
import java.util.concurrent.FutureTask;
import java.util.concurrent.Callable;
public class CreateCallableThread{
    public static void main(String[] args){
         FutureTask<String> task = new FutureTask<>(new Callable<String>(){

            @Override
            public String call()throws Exception {
                String lastName = "wu";
                String firstName = "zhongpeng";

                return lastName + firstName ;
            } 
         });

         Thread thread = new Thread(task);
         thread.start();
         try{
            String getName = task.get(); //获取线程返回值；有异常so要捕获
            System.out.println(Thread.currentThread().getName() + "call return String text is :" + getName);
         }catch(Exception e){
            System.out.println(e.getMessage());
         }
    }

}
╭─
```

##### 4.线程池：executorService;

###### 1.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public static void concurrentPrinting(){
        int threadCount = 10;
        ExecutorService executorService = Executors.newFixedThreadPool(threadCount);
        executorService.submit(new Runnable(){
            @Override
            public void run(){
                for(int i = 0 ; i< 100 ; i++){
                    System.out.println(Thread.currentThread().getName() + "--> " + i);
                    }
            }
});
        executorService.shutdown();

    }
}
```

###### 2.

```java
 import java.util.concurrent.ExecutorService;
 import java.util.concurrent.Executors;

          public static void concurrentPrinting(){
        int threadCount = 10;
        ExecutorService executorService = Executors.newFixedThreadPool(threadCount);
        for(int i = 0; i < threadCount ; i++){
            executorService.execute(()->{
                Singleton singleton = Singleton.getInstance(); 
                System.out.println(singleton);
            });
        }
        executorService.shutdown();

    }
}



 /**
 *  ExecutorService
 *  Executors.newFixedThreadPool()
 *  instance.shutdown()
 *  instance.submit();
 *
 */
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
//+++++++++++
public class MyThread extends Thread{

    public MyThread(String threadName){
        super(threadName);    //继承父类的字段... Thread("起线程名")；
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

- Terminated: 结束(释放)  ---> 主动and被动

##### 线程生命周期状态图

![](/home/administrator/.config/marktext/images/2024-10-01-14-50-49-image.png)

![](/home/administrator/.config/marktext/images/2024-10-01-15-40-56-image.png)

- 有限（非无限循环）的线程，执行完毕就销毁了，再次调用就调用不到了（得重新new申请空间）

-------

##### .sleep(); 的使用

> sleep的功能就是让<u>当前线程</u> 进行 <mark>有限时间(超时)等待</mark>...

> 1 s = 1000ms
> 
> - 线程使用 .sleep之后，会释放cpu执行（腾出来给其他线程使用），转而进入堵塞态(等待状态...wait多久，根据传入sleep的参数时间决定)
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
        System.out.println(".... dangdangdang !");

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

> .sleep的异常使用尝试捕获try-catch，<mark>不要</mark>在run方法进行 throws （不要在方法那里throws 抛出）InterruptedException [X]

![](/home/administrator/.config/marktext/images/2024-10-01-19-59-12-image.png)

##### .interrupt();  //直接将线程终止...  （实例方法）

> 终止 <mark>调用对象的</mark> 线程！：谁调用的终止谁的睡眠

```java
Thread thread = new Thread(new MyRunnable());


thread.interrupt();//sleep()是要捕获异常的，
                   // interrupt()执行，sleep()捕获到异常，就会抛出异常，实现sleep的关闭... 
```

有睡眠，想把正在睡觉中的线程提前叫醒：<mark>.interrupt( ); </mark> //实例方法...

##### ~~.stop():暂停当前调用的线程~~

- 使用一个标志属性 来控制！！

> 过时了
> 
> - stop() 的作用：强行终止线程(还没保存的文件直接丢失！！！)
> 
> - 取代方式（更好的方式：<mark>使用 属性，通过标志flag的boolean</mark>来决定要不要执行sleep...）
> 
> ```java
> //使用打标志，来实现stop： 在main主线程修改标志的值
> 
> 
> 
> //thread-1:
> public class MyRunnable implements Runnable{
>     boolean runFlag = true;//flag !!!！！！！！！！！！！！！！！！！！！！
>     @Override 
>     public void run(){
>         for(int i = 0; i< 18; i++){
>             if(runFlag){
>                 System.out.println(Thread.currentThread().getName() + "---> " + i);
>                 try{
>                     Thread.sleep(1000);
>                 }catch(InterruptedException e){
>                     e.printStackTrace();
>                     System.out.println("open interrupt ...");
>                  }
>             }else{
>                 return;//释放线程压入的run方法
>             }
>         }
>     }
> }
> 
> 
> // main-thread:
> package com.thread;
> public class SleepTimesThread{
>     public static void main(String[] args){
>         MyRunnable runnable = new MyRunnable();
>         Thread thread = new Thread(runnable);
>         thread.start();
>         try{
>             Thread.sleep(2000); //2秒后main线程休眠
>         }catch(Exception e){
>             e.printStackTrace();
>         }
>         runnable.runFlag = false;//2秒后苏醒，执行到该语句：将其关闭
>     }
> 
> 
> }
> ```

##### .join(); 阻塞当前所在位置的进程

> .join() 是实例方法；//该线程合并到主线程...
> 
> .sleep() 是静态方法；Thread.sleep();

```java
main(){
//Thread t = new MyThread();
Thread t = new Thread(new Runnable());
t.start();
t.join(); //阻塞 main 线程，等t线程执行完毕,main才会被唤醒回到就绪状态等待被cpu调度
//...
}
```

- join() 使用实例：

```java
public class JoinTest{
    public static void main(String[] args){
       Thread thread = new Thread(new MyRunnable());
       thread.start();
       try{
            thread.join();
       }catch(InterruptedException e){
            System.out.println(e.getMessage());
       }

       System.out.println(Thread.currentThread().getName() + " : start...");
            for(int i = 0; i < 18; i++){
                System.out.println(Thread.currentThread().getName() + "---> " + i);
            }
      System.out.println("end...");
    }


} 
 //------ new Thread --------------------
    class MyRunnable implements Runnable{
        @Override
        public void run(){
            for(int i = 0; i < 18; i++){
                System.out.println(Thread.currentThread().getName() + "---> " + i);
            }
        }
    }
```

- join也可设置时长，但是和sleep的不一样，join的时长设置，和它的线程程序执行有关，线程执行完毕，就会释放，不会像sleep一样，设置10秒,就必须休眠10秒

```java
Thread thread = new Thread(new Runnable();
thread.start();
thread.join(100); //...
```

---

#### 守护线程(Daemon)：

> 线程有两种： 用户线程、守护线程

- 特点：
  
  - 所有【用户线程】结束后，【守护线程】也会<mark>自动</mark>（无手动销毁动作）结束
  
  - GC线程就是一个经典的守护线程

- 守护线程的用途：
  
  - 数据库的定时备份

##### 如何设置  - 守护线程

```java
threadObj.setDaemon(true);
threadObj.setDaemon();//默认 false；
```

- 守护线程的run方法体一般都是 while(true){}...无限循环，保持后台一直监听...

![](/home/administrator/.config/marktext/images/2024-10-01-22-35-36-image.png)

---

#### 定时器(守护线程的方式)：Timer （了解即可...）

- Timer timer = new Timer(**true**);  //本质就是一个线程，<mark>加上true参数，就是守护线程</mark>

> Spring 框架 有实现定时功能...SpringTask（包装了java.util.Timer/java.util.TimerTask）

- java.util.Timer;

- java.util.TimerTask;

![](/home/administrator/.config/marktext/images/2024-10-01-22-36-59-image.png)

> ![](/home/administrator/.config/marktext/images/2024-10-01-23-07-28-image.png)

![](/home/administrator/.config/marktext/images/2024-10-01-22-37-18-image.png)

> 使用jdk提供的定时器：取代使用 Thread.sleep()来进行定时的操作

```java
Timer timetor = new Timer(true);

/*  tread-obj.schedule(task,start-date,sleep-time) */
timetor.schedule(new LogTimerTask(),new Date,1000); //从当前时间，每隔一秒执行LogtimerTask()；
//LogTimrTask()是需要实现TimerTask的抽象类的
```

![](/home/administrator/.config/marktext/images/2024-10-01-23-18-16-image.png)

![](/home/administrator/.config/marktext/images/2024-10-01-23-19-14-image.png)

---

也可使用“匿名内部类”的方式：实现 abstract  TimerTask()的 run（Override）：

![](/home/administrator/.config/marktext/images/2024-10-01-23-22-30-image.png)

     

-----

#### JVM调度（属于进程状态的  就绪 <——> 运行）

> JVM使用的是 抢占式调度
> 
> - 优先级高的得到cpu执行的概率就高(会抢占低优先级的线程)

##### 优先级调度：

> - 默认线程优先级是 5; Thread.NORM_PRIORITY
> 
> - 1 是最低； Thread.MIN_PRIORITY
> 
> - 10是最高；Thread.MAX_PRIORITY

###### getPriority():获取该线程的优先级：

```java
Thread thread = new Thread(new MyRunnable());
System.out.println(thread.getPriority());
```

###### setPriority():手动获取线程优先级

```java
Thread thread = new Thread(new Runnable());
Thread thread_2= new Thread(new Runnable());
thread.setPriority(Thread.MAX_PRIORITY);
//将该线程优先级设置到最高...
thread_2.setPriority(6); //手动设置到6的级别
```

###### 小实例：

```java
public class PriorityTest{
    public static void main(String[] args){
       Thread thread = new Thread(new MyRunnable());
       Thread thread_2 = new Thread(new MyRunnable());
       thread_2.setPriority(Thread.MAX_PRIORITY);
       thread.setPriority(Thread.MIN_PRIORITY);
       thread.setName("[-]min-thread");
       thread_2.setName("[+]max-thread+++");
       thread.start();
       thread_2.start();

    }
} 

    class MyRunnable implements Runnable{
        @Override
        public void run(){
            for(int i = 0; i < 18; i++){
                System.out.println(Thread.currentThread().getName() + "---> " + i);
            }
        }
    }
```

- 运行结果：

```shell
❯ vim JoinTest.java
❯ javac JoinTest.java
❯ java JoinTest
[+]max-thread+++---> 0
[+]max-thread+++---> 1
[+]max-thread+++---> 2
[+]max-thread+++---> 3
[+]max-thread+++---> 4
[+]max-thread+++---> 5
[+]max-thread+++---> 6
[+]max-thread+++---> 7
[+]max-thread+++---> 8
[+]max-thread+++---> 9
[-]min-thread---> 0
[+]max-thread+++---> 10
[+]max-thread+++---> 11
[+]max-thread+++---> 12
[+]max-thread+++---> 13
[+]max-thread+++---> 14
[+]max-thread+++---> 15
[-]min-thread---> 1
[+]max-thread+++---> 16
[+]max-thread+++---> 17   #级别高的优先被执行完毕
[-]min-thread---> 2
[-]min-thread---> 3
[-]min-thread---> 4
[-]min-thread---> 5
[-]min-thread---> 6
[-]min-thread---> 7
[-]min-thread---> 8
[-]min-thread---> 9
[-]min-thread---> 10
[-]min-thread---> 11
[-]min-thread---> 12
[-]min-thread---> 13
[-]min-thread---> 14
[-]min-thread---> 15
[-]min-thread---> 16
[-]min-thread---> 17
```

###### yield()://主动将cpu执行权让出，回到就绪状态（不是进入阻塞状态）

- yield() 是静态方法；

```java
Thread.yield(); //和sleep一样，放到哪里，哪个线程就让位
```

>     [      ]                [       ]
>     [readly]  ----------->  [running]
>     [      ]  <-- yield --  [       ]

![](/home/administrator/.config/marktext/images/2024-10-02-17-15-03-image.png)![](/home/administrator/.config/marktext/images/2024-10-02-17-15-30-image.png)

### 考虑线程安全问题    ： 很重要

![](/home/administrator/.config/marktext/images/2024-10-02-17-22-16-image.png)

> 局部变量的基本数据类型变量（在栈区不共享）不考虑有并发问题，但是引用类型： 是地址是在堆区，是资源共享的，有脏读写的问题

- 例子：

![](/home/administrator/.config/marktext/images/2024-10-02-17-27-34-image.png)

> 不加锁，(** 加锁**：线程同步机制-原子级操作  --- <mark>安全</mark>)就可以出现有一万取出两万三万...的风险... 
> 
> - 不加锁，多个线程各自干各自的，是 “线程异步机制-也就是 线程并发” ---<mark>效率</mark>

- 实例变量 和 静态变量都有 线程安全问题

---

#### 线程同步机制： synchronized

<mark>synchronized 的作用 就是给共享的对象 上锁</mark>

> 最好不要将同步机制作用到全局（也就是方法的 修饰上）
> 
> 范围越大，需要同步的就越多，就会导致性能下降
> 
> - 精确到需要同步的代码，使用 <mark>synchronized (){}  同步代码块</mark>；进行代码同步

```java
//加锁：lock
synchronized(需要同步操作的多线程所共享的对象:对象锁){
    //需要原子操作的部分
}
//解锁：unlock
```

###### 实例：

> 保护实例变量

```java
public class SycnchroizedTest{
    public static void main(String[] args){
       Account act = new Account("0306131313",10000);
       Thread thread_1 = new Thread(new WithDraw(act)); //线程1 和 线程2 使用同一资源，有脏读写的问题..
       Thread thread_2 = new Thread(new WithDraw(act)); 
       thread_1.start();
       thread_2.start();
    }

}

//-------------------------------------------------------------------
class WithDraw implements Runnable{
    private Account account; //共享资源 
    public WithDraw(Account act){ //线程的构造器
        this.account = act;
    } 
    @Override
    public void run(){
        account.withDraw(1000);
    }
}



//----------------------------------------------------------------------
class  Account{  //共享的类
    private String actNo;
    private double balance;

    public Account(String actNo, double balance){
        this.actNo = actNo;
        this.balance = balance;
    }


    public String  getActno(){
        return this.actNo; 
    }

    public double getBalance(){
        return this.balance;
    }
    public void setBalance(double balance){
        this.balance = balance;
    }
/*
    public synchronized void withDraw(double money){
        double before = this.getBalance();//获取当前的余额
        System.out.println(Thread.currentThread().getName() + "正在取钱，当前" + this.getActno()  + "的余额 ：" +  before);
        try{
           Thread.sleep(1000);
        }catch(InterruptedException e){
            System.out.println(e.getMessage());
        }
        this.setBalance(before - money);
        //this.balance = this.balance - money;
        System.out.println(Thread.currentThread().getName() + "取钱成功，当前余额 ：" +  this.getBalance());

    }
*/

//优化一下：缩小 synchronized 的范围：精确同步范围     
    public  void withDraw(double money){
        synchronized(this){ //找共享对象：关键；得是线程对象共享
                    double before = this.getBalance();//获取当前的余额
                    System.out.println(Thread.currentThread().getName() + "正在取钱，当前" + this.getActno()  + "的余额 ：" +  before);

                    this.setBalance(before - money);
                    //this.balance = this.balance - money;
                    System.out.println(Thread.currentThread().getName() + "取钱成功，当前余额 ：" +  this.getBalance());
        }
    }

}
```

###### 类锁：

static synchornized : 对同一类上锁  

> 保护静态变量

![](/home/administrator/.config/marktext/images/2024-10-02-23-19-42-image.png)  

> 这个实例虽然有两个对象，但是是同一个类；所以是同步的，需要等待

###### 单例模式-synchronized 实现： 双重校验锁

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
//--------------------------------------------------------------------
public class SingletonTest{

    public static void main(String[] args){
      // Singleton s1 = Singleton.getInstance(); 
      // Singleton s2 = Singleton.getInstance(); 
      // System.out.println(s1 == s2);
      // System.out.println(s2);
      // System.out.println(s1);
         concurrentPrinting(); //使用线程池创建线程
    }

    public static void concurrentPrinting(){
        int threadCount = 10;
        ExecutorService executorService = Executors.newFixedThreadPool(threadCount);
        for(int i = 0; i < threadCount ; i++){
            executorService.execute(()->{
                Singleton singleton = Singleton.getInstance(); 
                System.out.println(singleton);
            });
        }
        executorService.shutdown();

    }
}
//------------------------------------------------------------------



//单例模式 - 使用了 synchronized 做锁   -------------------------------
class Singleton{
    private static volatile Singleton singleton = null;
    private Singleton(){}
    public static Singleton getInstance(){
        if(singleton == null){
            synchronized(Singleton.class){
                if(singleton == null){
                    singleton = new Singleton(); 
                }
            }
        }
        return singleton;
    }
}
```

#### 线程同步的新方式： ReentrantLock （可重入锁）

- lock() //上锁

- unlock()//解锁

- 实现：

```java
import java.util.concurrent.locks.ReentrantLock;
...

private static final ReentrantLock lock = new ReentrantLock();/


lock.lock();
...原子操作：
lock.unlock();
```

#### 锁总结：

- synchronized : 对象锁

- ReentrantLock : lock() ; unlock() ：互斥锁(可重入锁 )
