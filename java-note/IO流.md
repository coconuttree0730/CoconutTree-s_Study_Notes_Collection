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

- 流，使用后要进行关闭(关水闸).close()

- ![](/home/administrator/.config/marktext/images/2024-09-24-21-40-12-image.png)

                       +-------+
       +---+___________|       |    使用.flush()，可以将缓冲区中的数据写入到磁盘(不是清空)
       | m |__buffer___|   d   |
       +---+           |       |
                       +-------+

---

### FileStream

- 字节读写

### FileInputStream

### FileOutputStream

- 字符读取

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
