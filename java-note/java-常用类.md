### (1)String

> 字符串 类
> 
> - String str = new String("hello world");  //[过时不推荐]
> 
> - String str = "hello world";
> 
> - "文本" ---> 运行时（<mark>一次初始化时扫描全部</mark>）  **加载进【字符串常量池 -  缓存机制 】同 【static 常量池一样】 且都在 heap中： 不用重复 new 对象**... 
>   
>   - String str1 = "hello" 
>   
>   - String str2 = "hello"
>   
>   - > System.out.printnl(str2 == str1); //  true ,地址相同，表示是访问的同一个  
>   
>   - ![](/home/administrator/.config/marktext/images/2024-08-22-17-40-29-image.png)  <mark>new String()</mark> 就是新开辟空间 所以就会不相等： 使用：equals（），所以，不推荐使用 new String("xxx"),的原因就是会new多个空间，消耗内存...
>     
>     ```java
>     String str1 = "hello";
>     String str2 = "world";
>     String str3 = str1 + str2;//底层是  StringBulider 类（使用 “+” 号）的时候(“+”号左右有一个是变量的时候)
>     // str3 指向 堆区，并不指向 常量池
>     //使用 intern（）,将堆区的“hello world”放入常量池
>     String internStr = str3.intern();
>     
>     String str4 = "helloworld";
>     System.out.prinln(str3 == str4)//false: 使用拼接成的字符串不会放入常量池
>     ```
>     
>     String name = "wu"  + "zhongpeng"; //编译阶段就： String name = “wuzhongpeng";运行阶段写入到池里的就是“wuzhongpeng”e

              ![](/home/administrator/.config/marktext/images/2024-08-22-19-52-22-image.png)

> - "xxxx" 是常量，存于string常量池，**不可变**，但 指向可变：就是说，叫wu,就是wu，你不能把的地址上的值改掉，但是它可以指向其他变量
>   
>   ```java
>   String name = "zhangshan";//“zhangshan”是字面量，不可变
>   name = "lishi"  //[可以]
>   // Stringbuilder ...  可变
>   ```

- 补充：

![](/home/administrator/.config/marktext/images/2024-08-22-22-47-53-image.png)

```java
String name = new String("zhangshan");   //[不推荐了] **********---
```

#### string - construction

```java
char[] chars = new char[]{'h','e','l','l','o'};
String strChar = new String(chars,0,2);//“从0位置开始，取两个长度输出”
String strChar = new String(chars);


byte[] buffer = new byte[]{32,45,32,1,244};
String strByte = new String(buffer);
String strByte = new String(buffer,2,2);//frist: , to: .
```

- 乱码：

```java
byte[] str = “hello,world”.getByte("GBK");

String str2 = new String(str,"UTF-8");
//"天下足球".getBytes(StandardCharsets.UTF_8);
//StandardCharsets.UTF_8 == "UTF-8"  <=== 常量
System.out.println(str2);
```

> 避免乱码发生：使用 charset.defaultcharset(); //默认本机的编码方式

```java
        byte[] bs = "天下足球".getBytes(Charset.defaultCharset());
        String defaultCharset = new String(bs, Charset.defaultCharset());
        System.out.println(defaultCharset);
```

String - function

- 常用方法
  
  - “find“

![](/home/administrator/.config/marktext/images/2024-08-22-22-57-45-image.png)

- - 截取：.substring(end) //默认0下标开始
    
    ![](/home/administrator/.config/marktext/images/2024-08-22-23-17-14-image.png)
    
    > ![](/home/administrator/.config/marktext/images/2024-08-22-23-17-40-image.png)

  

    中间截取：（substring(begin,end)）

![](/home/administrator/.config/marktext/images/2024-08-22-23-19-21-image.png)

- - 去除空白（trim   strip）

### (2)“可变长字符串“: <mark>StringBuilder</mark>

- <mark>StringBuffe</mark>r(线程安全)：出现的比Stringbuilder早
  
  > 保障安全必定耗时作安全处理（效率低）

- Stringbuilder
  
  > 效率高；适合不处理 并发处理的场景

### StringBuilder  VS   String  (大数据量处理性能pk)：

```java
public class strCat_string{
    public static void main(String[] args) {

        long start =  System.currentTimeMillis();

        //StringBuilder sb = new StringBuilder();
        String str = "";
        for(int i=0;i<100000;i++ ){
            //sb.append(i);//append(int);//@Overload
            str += i;
        }

        long end  =  System.currentTimeMillis();
        System.out.println("String excute the time is : " + (end - start));
    }

}
```

```java
public class strCat_stringbuilder{
    public static void main(String[] args) {

        long start =  System.currentTimeMillis();

        StringBuilder sb = new StringBuilder();

        for(int i=0;i<100000;i++ ){
            sb.append(i);//append(int);//@Overload
        }

        long end  =  System.currentTimeMillis();
        System.out.println("StringBuilder excute the time is  :" + (end - start));
    }

}
```

---

### (3)包装类

> Integer\Folat\Double... extends Number

- 拆箱and装箱
  
  - .**TypeElement**Value();  //拆 unboxing
    
    ```java
    Integer it = new Integer(12);//过时了：同String的  new String("XXX");
    
    int xxx = 42;
    Integer numToIneger = 12;   
    //or 
     Integer numToIneger_2 = Integer.valueOf(xxx);
    //新的赋值方式  也就是 【装箱】过程
    
    int num  = it_new.intValue();// num ===>  12  //【拆箱】
    double integerTodouble = it_new.doubleValue();
    //float xxx = ...floatValue();
    ```

#### Ineger(*)

- Integer 的缓存机制：
  
  > - （256：-128～127）提前new 放入常量池（是一个数组：Integer[] integercache ）;在范围内的整数之间池里取：so
  >   
  >   ```java
  >   Integer x = 10000;
  >   Integer y = 10000;
  >   // x == y : false 
  >   
  >   Integer xx = 127;  //同一个地方取的，地址肯定相同    
  >   Integer yy = 127;
  >   // xx == yy : true
  >   ```

##### 常用方法：

- parseInt();
  
  ```java
  String inputNum = scanner.next(); 
  int number = Ineger.parseInt(inputNum)
  //parseInt ====> C#: ConvertInt32(inputNum);
  ```
  
  ```java
  String str = Integer.toString(10086);//"10086"
  //or 
  Integer num = 666;
  String str_2 = String.valueOf(num);
  ```

#### 类型转换(图)：

- 转换实例：
  
  ```java
  //...
  ```

![](/home/administrator/.config/marktext/images/2024-08-24-17-36-36-image.png)

- 自动装箱/拆箱
  
  ```java
  Integer x = 10000;//boxing : I
  int y = x;
  ```

- 大 数字处理(整数)：
  
  ```java
  long l = 9999999999999999999999L; //[X]
  BigInteger bi = new BigInteger(“9999999999999999999999999999“);
  
  bi.add(23)//  23 + 99999999999999999999999999999    
  // ....
  ```

- 大数字（浮点）: java.math.BigInteger;
  
  ```java
  double b = 9999999999999999999999.99999   //【X】
  BigDecimal bd = new BigDecimal(“324.323”);
  
  BigDecimal bmal = new BigDecimal("123456789");
   bmal.movePointLeft(2)
   //1234567.89
   bmal.add("23"); //1234567.89 + 23
  ```

- 数字 格式化（ DecimalForm ）

> import java.text.DecimalFormat; //导入包

```java
DecimalFormat df = new DecimalFormat("###,###.##");
df.format(2334.4546);// "2,334.45"

DecimalFormat df = new DecimalFormat("###,###.0000");
df.format(13432.2); //13,432.2000
```

### 日期api

> java.util.Date:
> 
> ![](/home/administrator/.config/marktext/images/2024-08-25-00-30-02-image.png)

```java
Date date = new Date();
//Sun Aug 25 00:27:23 CST 2024
//等同于： System.currentTimeMillis();

long times = 1000;
Date date = new Date(times)
//Thu Jan 01 08:00:01 CST 1970
```

- 格式化日期：
  
  > - **import java.text.SimpleDateFormat;**
  > 
  > - **import java.util.Date;**

```java
Date now  = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
//SimpleDateFormat("yyyy/MM/dd HH:mm:ss SSS"); 2024/08/25 ...
sdf.format(now);
//"2024-08-25 00:41:48 386"
```

- String ---> Date（SimpleDateFormat）
  
  > .parse()
  
  ```java
  String time = "2024-08-25";
  SimpleDateFormat sdf_2 = new SimpleDateFormat("yyyy-MM-dd");//* 要与String匹配
  sdf_2.parse(time)
  //Sun Aug 25 00:00:00 CST 2024
  ```

### 日历api

> Calendar

```java
import java.util.Date;
import java.util.Calendar;
public class calendar{
    public static void main(String[] args) {

         Calendar cal = Calendar.getInstance();
         System.out.println(cal);

         System.out.println("---------------------");

         System.out.println(cal.get(Calendar.YEAR));
         System.out.println(cal.get(Calendar.MONTH));
         System.out.println(cal.get(Calendar.DAY_OF_MONTH));                
            //Calendar.YEAR
            //Calendar.XXXXX
            //..............
   }

 }
```

### java-8 （Date-Time）新特性

> 目的：线程安全(纳秒级)

> improt java.<mark>time</mark>.XXX;

##### (1)LocalDataTime()

###### .now()

```java
//获取当前时间(纳米)：
jshell> LocalDateTime now = LocalDateTime.now()
now ==> 2024-08-26T23:08:57.670603005
//毫秒级：jshell> System.currentTimeMillis()
//        $4 ==> 1724685119760
```

###### .of("y",'M',"d","h","m",'s','nanos');

```java
jshell> LocalDateTime now = LocalDateTime.of(2021,9,1,23,59,59,1)
now ==> 2021-09-01T23:59:59.000000001
```

###### 修改时间-日期：

- plusXXX() //加（可链式调用）

```java
// plus
// plusDay
// plusHours
// plusMinutes
// ...Seconds
// ...Weeks
// ...Years
// ...

jshell> LocalDateTime now = LocalDateTime.of(2021,9,1,23,59,59,1)
now ==> 2021-09-01T23:59:59.000000001

jshell>  LocalDateTime alter = now.plusYears(2).plusMinutes(34)
alter ==> 2023-09-02T00:33:59.000000001
```

##### (2)时间戳：

```java
jshell> Instant  timesamp  = Instant.now()
timesamp ==> 2024-08-26T15:26:00.549167943Z

//【***】
jshell> long times = timesamp.toEpochMilli()
times ==> 1724685960549
//long start =  System.currentTimeMillis();
```

##### (3)时间矫正器（TemporalAdjusters）

![](/home/administrator/.config/marktext/images/2024-08-26-23-28-57-image.png)

#### (4)格式化（新）：

> .ofPattern("")

![](/home/administrator/.config/marktext/images/2024-08-26-23-31-47-image.png)

#### (5)数学类：Math

![](/home/administrator/.config/marktext/images/2024-08-26-23-35-21-image.png)

#### (6)enum（引用数据类型）

- 继承 ： java.lang.Enum父类，final修饰
  
  ![](/home/administrator/.config/marktext/images/2024-08-26-23-43-01-image.png)
  
  - 枚举类型可遍历： enumNameXXX.values()
    
    ![](/home/administrator/.config/marktext/images/2024-08-26-23-44-38-image.png)
  
  ---

> piblic enum Season{
> 
>     XXX,
> 
>     YYY,
> 
>     ZZZ
> 
> }

   ![](/home/administrator/.config/marktext/images/2024-08-26-23-38-16-image.png)

> ![](/home/administrator/.config/marktext/images/2024-08-26-23-39-37-image.png)

---

- 应用：
  
  ![](/home/administrator/.config/marktext/images/2024-08-26-23-39-12-image.png)
  
  ###### enum的高级用法：
  
  1.和类一样：类有的它都有；
  
  2.构造方法，enum体内 “new”
  
  ![](/home/administrator/.config/marktext/images/2024-08-26-23-48-18-image.png)
  
  不使用 new...!
  
  ![](/home/administrator/.config/marktext/images/2024-08-26-23-48-40-image.png)
  
  > 访问enum的成员： enumName.CONDTNAME.getXXX();

- 实现接口的方式：
  
  1. (enum体内)![](/home/administrator/.config/marktext/images/2024-08-26-23-52-20-image.png)
  
  2. （enum常量成员体内）![](/home/administrator/.config/marktext/images/2024-08-26-23-52-52-image.png)

#### (7)Random

> java.util.Random;

![](/home/administrator/.config/marktext/images/2024-08-26-23-55-41-image.png)

#### (8)System类：

> import java.lang.System;

![](/home/administrator/.config/marktext/images/2024-08-26-23-57-45-image.png)

###### System.out <===> <mark>PrintStream</mark>

![](/home/administrator/.config/marktext/images/2024-08-26-23-59-30-image.png)

###### System.in <===> <mark>InputStream</mark>

![](/home/administrator/.config/marktext/images/2024-08-27-00-00-30-image.png)

###### error-错误流输出：（红色提示）

> - 用于 catch处理异常的错误输出正合适

![](/home/administrator/.config/marktext/images/2024-08-27-00-01-21-image.png)

###### System.nanoTime()

> 获取**纳秒**时间戳

###### System.getenv()

> - 获取环境变量：
>   
>   ![](/home/administrator/.config/marktext/images/2024-08-27-00-05-00-image.png)

###### 获取系统属性：

System.getProperties()//@return Properties

System.getProty("java.vm.name")//@return String

#### (9)UUID

**<mark>UUID.randUUID() ;  TYPE  : UUID</mark>**

![](/home/administrator/.config/marktext/images/2024-08-27-00-10-20-image.png)

![](/home/administrator/.config/marktext/images/2024-08-27-00-10-45-image.png)

 ![](/home/administrator/.config/marktext/images/2024-08-27-00-10-58-image.png)

- 操作：   

```java
import java.util.UUID;


UUID uuid = UUID.randomUUID();//对象uuid
//e05563d2-d095-4e2b-8c07-a7090738603f

//-----------------------------------------

/*tostring*/
String s = uuid.toString();
String handle = s.replace("-","");
// handle: "e05563d2d0954e2b8c07a7090738603f"
```
