## IO流

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

- 流，使用后要进行关闭(关水闸)<mark>.close()</mark>，所有流都实现了 Cloaseable接口![](/home/administrator/.config/marktext/images/2024-09-25-16-40-49-image.png)
  
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

> 使用 FileInputStream 和 FileOutputStream 实现 <mark>**文件复制**</mark>
> 
> - 关键代码：
> 
> ```java
> while((readCount = in.read(buffer)) != -1){
>       out.write(buffer,0,readCount);
> }
> 
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



### FileReader

### FileWriter

### 缓冲流（Buffer...）

### 转换流（InputStreamReader   \   OutputStreamWriter）

### 数据流  (Data...)

> 把数据类型也进行读写

### 对象流

>  (Object...把jvm对象从硬盘中存取(读写）)

### 打印流 (Print...)

---

- 标准输入输出

### System.out

![](/home/administrator/.config/marktext/images/2024-09-24-19-39-41-image.png)

> so : <mark>System.out</mark> === <mark>InputStream</mark>

### System.in

### System.err

### File类

### 装饰器：设计模式

### 压缩and解压流

### 字节数组流

### 对象克隆

- 深浅...克隆...
