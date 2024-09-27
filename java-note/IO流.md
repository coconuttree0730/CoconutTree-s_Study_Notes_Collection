## IO流

> 关于 io 说明：
> 
> - 操作文件，适合使用 <相对路径>：因为部署到服务器，使用<绝对路进>就会失去了灵活可移植性

###### IO流概念(图)：

![](/home/administrator/.config/marktext/images/2024-09-24-21-03-15-image.png)

---

###### IO流文件结构(图)：

- 常用（颜色一样是一对）    ![](/home/administrator/.config/marktext/images/2024-09-24-21-04-25-image.png)

> ![](/home/administrator/.config/marktext/images/2024-09-24-21-30-00-image.png)![](/home/administrator/.config/marktext/images/2024-09-24-21-30-11-image.png)![](/home/administrator/.config/marktext/images/2024-09-24-21-30-25-image.png)![](/home/administrator/.config/marktext/images/2024-09-24-21-30-51-image.png)![](/home/administrator/.config/marktext/images/2024-09-24-21-31-17-image.png)

![](/home/administrator/.config/marktext/images/2024-09-24-20-49-45-image.png)

使用 字节流容易出现乱码...因为不是一整个字符获取，所以会出现只获取某个字符的一部分...

- 字节流： 图片、音频、视频... 

- 字符流：文本...  (不适合读图片...二进制形式文件)

- **输入and输出** 是<mark>相对 于 内存</mark>() 来形容的....

![](/home/administrator/.config/marktext/images/2024-09-24-19-55-59-image.png)  

> 相当于大脑，外界读取(read)信息进入大脑，想长时间记住，就需要记录(wrIte)到纸上

- 处理流 and 结点流
  
  > 处理流【结点流【】】

###### IO流注意事项

- 流，使用后要进行关闭(关水闸)<mark>.close()</mark>，所有流都实现了 Closeable接口![](/home/administrator/.config/marktext/images/2024-09-25-16-40-49-image.png)
  
  ![](/home/administrator/.config/marktext/images/2024-09-25-19-46-17-image.png) 
  
  > AutoCloseable --> Closeable --> InputStream...
  
  - 表示可关闭的(流用完不关闭，会占用资源)

---

- ![](/home/administrator/.config/marktext/images/2024-09-24-21-40-12-image.png)
  
       +---+___________|       |    使用.flush()，可以将缓冲区中的数据写入到磁盘(不是清空)
       | m |__buffer___|   d   |
       +---+           |       |
  
                       +-------+

---

### FileStream

- 字节读写

### FileInputStream

- InputStream in = new FileInputStream();

- in.read();  //@return char\ -1

- in.read(byte[] by);  //@return readSize \ -1

- in.read(byte[], off ,size); //...

- in.skip( n );  //跳过n字节

- in.close() ; 

- <mark>**in.available(); //获取文件中剩余该byte个数...**</mark>

```java
byte[] buffer = new byte[in.available()]   //关键代码...

//具体代码：
import java.io.*;
public class ReadFileTest {
    public static void main(String[] args) {
        InputStream fis = null;
        String fileName = "./demo.java";
        try {
            // 读取文件
            fis = new FileInputStream(fileName);
            byte[] buffer = new byte[fis.available()]; //byte[] buffer = new byte[1024];
            int size = 0;
            System.out.println("inputstream-demo..");
            while ((size = fis.read(buffer)) != -1) {
                System.out.print(new String(buffer,0,size));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            if(fis != null){
                try{
                        fis.close();
                }catch(IOException e){
                    e.printStackTrace();
                }

            }


        }
    }
}
```

### FileOutputStream

- byte[]

- 字节写:

![](/home/administrator/.config/marktext/images/2024-09-25-18-18-04-image.png)

- FileOutputStream(xxx, true) //追加  ===> File * fp = fopen("./path/...",aw);

- FileOutputStream(xxx,不传入第二个参数，默认false)
  
  ###### 默认
  
  ###### 常用方法：
  
  ![](/home/administrator/.config/marktext/images/2024-09-25-18-20-55-image.png)

- 补充： 

```java
byte[] buffer = new byte[size];
buffer = "hello java".getBytes(); //将string转换成byte[] :buffer--> [97][23][23]...
out.write(buffer) 
```

###### 小实例：

> 使用 FileInputStream 和 FileOutputStream 实现 <mark>**文件复制**</mark>  能复制文本及其其他文件(图片 ，视频, ppt ,.......)
> 
> - 关键代码：
> 
> ```java
> int readCount = 0;
> byte[] buffer = new byte[in.available()];
> while((readCount = in.read(buffer)) != -1){
>       out.write(buffer,0,readCount);
> }
> ```

```java
/**
 * @author : administrator
 * @created : 2024-09-25
**/
import java.io.*;
public class OutputInputStreamCopy{
    public static void main (String[] args) {
        String fileName = "./ReadFileTest.java";
        String toFileName = "./OutputInputStreamCopy.text";

        InputStream in = null;
        OutputStream out = null;
        try{
            in = new FileInputStream(fileName);
            out = new FileOutputStream(toFileName);
            int readCount = 0;
            byte[] buffer = new byte[in.available()]; 
            //后面学习到 BufferedInputStream...可以使用来替代自定义byte[]
            while((readCount = in.read(buffer)) != -1){
                out.write(buffer,0,readCount);
            }

            out.flush();
            System.out.println("copy success !");
        }catch(IOException e){

            e.printStackTrace();
            throw new RuntimeException();
        }finally{
            if(out !=  null){
                try{
                    out.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if(in !=  null){
                try{
                    in.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }


        }
    }


}
```

- 补充：java-version 7之后，有 try - with - resources 特性... 

> try( resources ){}   ==> 取代 finally{ .close() ...}
> 
> ![](/home/administrator/.config/marktext/images/2024-09-25-19-46-17-image.png)
> 
> - <mark>凡是继承了 AutoCloseable接口的，都可以使用 try-with-resources</mark>![](/home/administrator/.config/marktext/images/2024-09-25-19-50-24-image.png)
> 
> - 实例：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-25-20-03-15-image.png)

---

- 》》》》》 FileReader \ FileWriter

- 读写文件文本 使用 字符流比 字节流好使（不容易出现乱码...）

- 和前面的File... Strem 的区别：<mark>char[ ]</mark>，还有就是 有一个 apend方法（<mark>追加</mark>）
  
  - .append(‘char’);    
  
  - .append("string") 

### FileReader ： 解码

> 字符流 读到内存
> 
> ![](/home/administrator/.config/marktext/images/2024-09-25-20-37-30-image.png)
> 
> - 和 FileinputStrem ...一样...
> - .skip(num) 是跳过num个字符

###### FileReader ... 解码方式

- 默认使用UTF-8 字符集编码

![](/home/administrator/.config/marktext/images/2024-09-26-16-19-12-image.png)

![](/home/administrator/.config/marktext/images/2024-09-26-16-25-31-image.png)

### FileWriter ： 编码

> （磁盘保存的是二进制，将文件编码成二进制放入磁盘）-- 默认 utf-8

> 字符流 写到磁盘
> 
>     ![](/home/administrator/.config/marktext/images/2024-09-25-20-38-19-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-26-16-28-52-image.png)

###### 小实例：FileWriter + FileReader  实现 copy：

```java
/**
 * @author : administrator
 * @created : 2024-09-25
**/
import java.io.*;
public class read_write_copy{

    public static void main (String[] args) {
        String toFileName = "./reader_writer_copy.txt";
        String fileName = "./FileStream/fileinputstream/demo.java";
        try(FileReader in = new FileReader(fileName);
            FileWriter out = new FileWriter(toFileName)){
                int readCount = 0;
                char[] chars = new char[512];
                //.read(string)
                //.read(char)
                //.read("hello".toCharArray())
                while((readCount = in.read(chars)) != -1){
                    out.write(chars,0,readCount);
                    out.append("\n");
                }
                System.out.println("copyed success!");
                out.flush();
            }catch(IOException e){
                e.printStackTrace();
            }
    }

}
```

---

---

### 缓冲流（Buffer...）--> <mark>效率高</mark> 适合操作大文件

- buffered...和xxxputStream的区别：
  
  - buffered是(操作)读取内存中的缓冲区byte[]数组...
  
  - xxxputStream是直接读写【内存 -- 磁盘】：内存和磁盘的速度是有差异的,所以，就造成了速度慢(跟不上)
  
  - BufferxxxStream 和 FilexxxStream 是一对
  
  - BufferReader 和 FileReader 是一对

##### BufferedInputStream：将InputStream<mark>包装</mark>起来

> 用法和 FileInputStream...是一样的api...

![](/home/administrator/.config/marktext/images/2024-09-25-22-38-39-image.png)

> 节点流：端到端
> 
> 处理流： 对节点的包装：如上图，bufferedInputStream就是包装了 InputStream
> 
> BufferedXXX ，就是官方给一个现成的“byte[] buffer ...”的意思![](/home/administrator/.config/marktext/images/2024-09-25-22-44-05-image.png)
> 
> - 使用：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-25-22-50-38-image.png)
> 
>   ![](/home/administrator/.config/marktext/images/2024-09-25-22-53-07-image.png)
> 
> - 关闭时，只需要关闭包装的处理流即可，因为封装了InputStream的.close()..,所以，关闭处理流就一起将节点流也关闭了
> 
> ![](/home/administrator/.config/marktext/images/2024-09-25-22-48-16-image.png)

##### BufferedOutputStream

...

###### buffered...实例：

```java
/**
 * @author : administrator
 * @created : 2024-09-25
**/
import java.io.*;
public class BufferedCopy{

    public static void main (String[] args) {
        long start = System.currentTimeMillis();
        try(BufferedInputStream read = new BufferedInputStream(new FileInputStream("./HelloWorld.java"));
            BufferedOutputStream write = new BufferedOutputStream(new FileOutputStream("./Copy_HelloWorld.java"))){
                int readCount = 0;
                byte[] buffer = new byte[1024];
                while((readCount = read.read(buffer)) != -1){
                    write.write(buffer,0,readCount);
                }
                write.flush(); 
        }catch(IOException e){
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("time: " + (end - start) + " ms");
    }
}
```

##### Buffered<mark>Reader</mark>

> - bufferedReader + FileReader
> 
> - bufferedReader + FileInputStream

![](/home/administrator/.config/marktext/images/2024-09-25-23-26-32-image.png)

```java
/**
 * @author : administrator
 * @created : 2024-09-26
**/
import java.io.*;
public class BufferReaderTest{

    public static void main (String[] args) {
        try(BufferedReader br = new BufferedReader(new FileReader("file.txt"))){
            String str = null; //temp...
            //.readLine()  //@ereturn null/str-line
            while((str = br.readLine()) != null){
                System.out.println(str);
            }            
        }catch(IOException e){
            e.printStackTrace();
        }
    }

}
```

---

- reset() ：退回到断点处(mark)

- mark()  : 在当前读到的位置打断点

现 mark  后 reset

         +------------------+
         |                  | 
         V                  |
     a   b  c  d  e   f  .  |
         |  ............. reset
         V  
        mrak

----

##### BufferedWriter

...

###### Buffered + Filexxxer 实例：

-----------

### <mark>转换流</mark>（InputStreamReader   \   OutputStreamWriter）

[字节流]  ———转换流———> [字符流]

###### 关于乱码：

- 文件编码格式和 程序编码格式不统一，就会导致出现乱码...

- 看到文件出现乱码时，优先想到是编码解码不匹配导致的 

> 能解决乱码问题    
> 
> - InputStreamReader： 解码(读出)
> 
> - OutputStreamWrirer: 编码(写入)

![](/home/administrator/.config/marktext/images/2024-09-26-20-56-32-image.png)

字节流：InputStream(节点)   +  字符流： Reader(包装)

![](/home/administrator/.config/marktext/images/2024-09-26-21-02-02-image.png)

> - Reader isr = new InputStreamReader(new **<u>FileInputStream</u>**("file-path"), <mark>charset_选择解码成什么字符集</mark>);
> 
> - FileReader() --->  封装了 InputStreamReader()
>   
>   - FileReader(''file-path',<mark> Charset.forName(</mark>"GBK")');
>   
>   - 如果FileReader(''....'',null)，没有选择字符集，会有出现乱码的情况

```java
/**
 * @author : administrator
 * @created : 2024-09-26
**/
import java.io.*;
public class InputStreamReaderTest{
    public static void main (String[] args) {
       try(Reader read = new InputStreamReader(new FileInputStream("testFile.java"))，GBK”){
            //Reader = read = new FileReader("file-path",Chatset.forName("GBK");
            char[] ch = new char[512];
            int readCount = 0;
            while((readCount = read.read(ch)) != -1){
                System.out.println(new String(ch,0,readCount));
            }
       }catch(Exception e){
            e.printStackTrace();

      } 
    }

}
```

- outputStreamWriter();

```java
/**
 * @author : administrator
 * @created : 2024-09-26
**/
import java.io.*;
//import java.nio.*;

public class OutputStreamWriterTest{
    public static void main (String[] args) {
        try( FileWriter fw = new FileWriter("testFile.java",true)){
               fw.write("test...");
        }catch(Exception e){
               e.printStackTrace();
            }

    }

}
```

------------

### 数据流  (Data...): 传数据实际内存存储的二进制

import java.io.*;

> 把数据类型进行读写：
> 
> - int num = 8 ; 在内存中什么状态，文件内就是什么状态 ： 二进制文件...上述（FileStream...OutputStream...）都是String...,不一样！！！

###### DataOutputStream(OutputStream out)

```java
DataOutputStream dos = new DataOutputStream(new FileOutputStream("file"));
int i = 23;
double d = 3.14
float f = 23.23f


dos.writeInt(i);
dos.writeDouble(d);
dos.weiteFloat(f);
//存到文件中的都是二进制形式，直接看，是乱码


dos.flush()
dos.colse()
```

###### DataInputStream(InputStream in): 读取使用dataoutstream写入的内容

```java
DataInputStream dis = new DataInputStream(FileInputStream(。。。))；
//因为存入时是有序的，so，读出也要有序,,,
//dos.writeInt(i); 
int i = dis.readInt();
//dos.writeDouble(d);
double d = dis.readDouble();
//dos.weiteFloat(f);
float f = dis.readFloat();

System.out.println(i);
System.out.println(d);
System.out.println(f);



dis.colse(); 
```

### 对象流： 序列化and反序列化: 传对象的二进制...

> 分布式系统！！！！！！应用：
> 
> - A服务器的jvm 调用 B服务器的 某个对象

>  (Object...把jvm对象从硬盘中存取(读写）)

- ObjectOutputStream     ObjectInputStream...

- java对象，在jvm内都是以二进制形式运行的,,,

```
// ObjectOutputStream
+---------------+
|   new ...     |   ---->  0100101001...   //序列化 ： 
+---------------+          (可 网络传输)

// ObjectInputStream
                    +-----------------+
10101010100  --->   |   new ....      |    //反序列化
                    +-----------------+
```

```java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream()）；

//包装（处理）流
```

- 一个序列化小实例：

```java
/**
 * @author : administrator
 * @created : 2024-09-26
**/
import java.io.*;
import java.util.*;

public class ObjectOutputStreamTest{
    public static void main (String[] args)  {
        try(ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("./object"));
){

        Date nowTime = new Date();//对象
        oos.writeObject(nowTime);//将对象序列化...写入到指定位置

        System.out.println("success");
        oos.flush();

}catch(IOException e){
    e.printStackTrace();
}        

    }


}
```

![](/home/administrator/.config/marktext/images/2024-09-26-23-22-50-image.png)

###### 序列化and反序列化(图解)：

![](/home/administrator/.config/marktext/images/2024-09-26-23-20-05-image.png)

- 反序列化：

![](/home/administrator/.config/marktext/images/2024-09-26-23-20-59-image.png)

- Date 类 实现序列化实例：ObjectOutputStream

```java
/**
 * @author : administrator
 * @created : 2024-09-27
**/
import java.io.*;
import java.util.*;
public class ListObject{

    public static void main (String[] args) throws Exception {
        String fileName = "./listObject";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(fileName));
        List<Date> now = new ArrayList<>();//通过 new 生成了对象(在内存中的形式)，才可序列化...
        now.add(new Date());
        now.add(new Date());
        now.add(new Date());
        now.add(new Date());
        now.add(new Date());
        now.add(new Date());

        oos.writeObject(now);  //**************
        oos.flush();
        oos.close();
        System.out.println("success");
    }
}
```

> .readObject();  //@return Object-value

![](/home/administrator/.config/marktext/images/2024-09-26-23-23-40-image.png)

```java
/**
 * @author : administrator
 * @created : 2024-09-27
**/
import java.io.*;
import java.util.*;
public class ListObjectRead{

    public static void main (String[] args)throws Exception {
       ObjectInputStream ois = new ObjectInputStream(new FileInputStream("listObject"));
       List<Date> list = (List<Date>)ois.readObject(); //要使用相同的类型接收
       Iterator<Date> it = list.iterator();
       while(it.hasNext()){
            Date now = it.next();
            System.out.println(now);
       }

    }
}
```

>  xxx.readObject();  //@return  对象内容读入到内存...

###### 注意：<mark>自定义的类</mark>要序列化需要  <u>implements</u> <mark>Serializable</mark> 接口...

![](/home/administrator/.config/marktext/images/2024-09-27-11-59-39-image.png)

> 同 拷贝... 之前学到的要进行对象深浅拷贝，要实现cloneable接口...    
> 
> - Serializable是标记接口(空的接口！！)：同 cloneable一样，只是标记作用

###### 关于 serializable接口：

![](/home/administrator/.config/marktext/images/2024-09-27-17-13-23-image.png)

- 会生成一个 seriaversionUID 编号... "对当前版本打上标记"

- **<mark>后续该类进行修改(添加属性...添加方法,,,)，该版本编译生成的UID是不一样的！！！</mark>**

- ![](/home/administrator/.config/marktext/images/2024-09-27-20-10-31-image.png)

![](/home/administrator/.config/marktext/images/2024-09-27-20-13-57-image.png)

- 类名 + 序列化版本号 ： srialversionUID

- 因为 Serializable 接口是打标记，使用：(常量)![](/home/administrator/.config/marktext/images/2024-09-27-20-20-25-image.png)
  
  可将该类版本锁定... （修改类内容，版本号不变，所以能继续执行反序列化）

> @Serial注解：检查版本号合法
> 
> ![](/home/administrator/.config/marktext/images/2024-09-27-20-31-23-image.png)

###### transient 避开 序列化：

- 使用 transient 做修饰

![](/home/administrator/.config/marktext/images/2024-09-27-20-34-04-image.png)

> 上述的 `age` 使用 `transient`修饰，之后，对该类进行序列化时，不会包含 `age` 属性...

### 打印流 (Print...)

##### PrintStream（字节形式）---> 非重点...

![](/home/administrator/.config/marktext/images/2024-09-27-21-04-12-image.png)

###### 优点：

![](/home/administrator/.config/marktext/images/2024-09-27-21-02-50-image.png)

###### 打印到指定文件

![](/home/administrator/.config/marktext/images/2024-09-27-21-06-03-image.png)

![](/home/administrator/.config/marktext/images/2024-09-27-21-06-21-image.png)

##### PrintWriter （字符形式）

![](/home/administrator/.config/marktext/images/2024-09-27-21-15-09-image.png)

- 关于构造函数, PrintStream,只能是 放入OutputStream(//字节输入流) ![](/home/administrator/.config/marktext/images/2024-09-27-21-25-29-image.png)

- PrintWriter(可以是 OutputStream [or] Writer)![](/home/administrator/.config/marktext/images/2024-09-27-21-57-45-image.png)![](/home/administrator/.config/marktext/images/2024-09-27-21-58-31-image.png)![](/home/administrator/.config/marktext/images/2024-09-27-21-58-44-image.png)

![](/home/administrator/.config/marktext/images/2024-09-27-22-00-12-image.png)

> 加上 true参数，可省去使用 .flush()的过程

---

^^^上述的都是普通输入输出流

### 标准输入输出

> 控制台 获取输入输出

![](/home/administrator/.config/marktext/images/2024-09-27-22-02-23-image.png)

### System.out

![](/home/administrator/.config/marktext/images/2024-09-24-19-39-41-image.png)

> so : <mark>System.out</mark> === <mark>PrintStream</mark>

```java
PrintStream ps = System.out;
ps.println("string...");
ps.println("hello java");



//改变输出源

System.setOut(new FileOutputStream("testFile.txt");
System.out.println("one");
System.out.println("two");
//...

// testFile.txt :
// one
// two
//...
```

### System.in  （不需要手动关闭）

- BufferedReader(System.in);

- Scanner(System.in);

![](/home/administrator/.config/marktext/images/2024-09-27-22-09-10-image.png)

```java
InputStream in = System.in;//控制台获取
InputStream in = new FileInputStream("fileName"); //文件or网络获取输入

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
Scanner scanner = new Scanner(System.in);//控制台获取输入
```

- 改变获取源：（没必要，使用 FileInputStream()就可以） System.setIn() //改变获取源

![](/home/administrator/.config/marktext/images/2024-09-27-22-18-40-image.png)

### System.err

-------

### File类

------

### 装饰器：设计模式

------

    

### 压缩and解压流

### 字节数组流

### 对象克隆

- 深浅...克隆...
