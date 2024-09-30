# 线程-并发

## 概述

### 线程执行内存图

#### 线程的实现方式：

##### 1. extends Thread  (和线程有血缘关系：is thread...)

> Override : public void run(){}

- Mythread:

```java
/**
 * @author : administrator
 * @created : 2024-09-30
**/
package com.thread;
public class MyThread extends Thread{
    @Override
    public void run(){
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

##### 2. implements Runnable （更推荐，还能“多继承“其他接口）

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
            new Thread(new Runnable(){
            @Override
            public void run(){
                for(int i = 0 ; i < 100 ; i++){
                    System.out.println("Runable-thread ---> " + i);
                }
            }
        }).start(); //匿名线程：RUnnable实现...


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
        super(threadName);    
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

> static Thread currentThread(); 

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
