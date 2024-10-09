## 网络编程：就是io流的应用

> 服务器，如： tomcat，已经将网络通信底层封装好；
> 
> 网路编程--->了解即可

- 因为是使用的io流，所有可以传输各种类型的数据！！！   

> 网络编程 三要素：
> 
> - ip
> 
> - 端口号(port) ; 2Byte: ( 0~65535)
> 
> - 传输协议: tcp\udp\...

- ip + 端口号 = socktet

- 发送 请求request 的叫【客户端】

- 接收并响应response 的叫 【服务端】

#### InetAddress类： 网络编程基础类

> import java.net.InetAddress;
> 
> sockte也是基础InetAddress 类,,,

![](/home/administrator/.config/marktext/images/2024-10-08-20-15-47-image.png)

> 就 4 个比较重要的

- 获取本地计算机的ip地址和域名

```java
import java.net.InetAddress;
public class InetAddressTest{
    public static void main(String[] args)throws Exception{
        InetAddress inetAddress = InetAddress.getLocalHost();//获取到本地计算机ip地址和域名 打包的对象
        System.out.println(" object: " + inetAddress);
        System.out.println(" address: " + inetAddress.getHostAddress());
        System.out.println(" DNSname: " + inetAddress.getHostName());
//------------------------------------------------------------------------
        System.out.println("-------------------------------------");
        InetAddress otherHost = InetAddress.getByName("www.baidu.com");//指定网络计算机
        System.out.println(" otherHost-object: " + otherHost);
        System.out.println(" address: " + otherHost.getHostAddress());
        System.out.println(" DNSname: " + otherHost.getHostName());

    }
}
```

```shell
❯ java InetAddressTest
 object: wuzhongpeng-thinkpad-x220/10.12.9.227
 address: 10.12.9.227
 DNSname: wuzhongpeng-thinkpad-x220
-------------------------------------
 otherHost-object: www.baidu.com/39.156.66.14
 address: 39.156.66.14
 DNSname: www.baidu.com
```

### URL ：统一资源定位符

> 定位网络上的某一资源
> 
> - 互联网上的每个文件都有唯一的URL
> 
> - so，使用URL可以定位文件位置，进行访问和..CRUD

![](/home/administrator/.config/marktext/images/2024-10-08-21-31-11-image.png)

#### java-URL类

> import java.net.URL;

![](/home/administrator/.config/marktext/images/2024-10-08-21-32-26-image.png)

![](/home/administrator/.config/marktext/images/2024-10-08-21-33-36-image.png) 

![](/home/administrator/.config/marktext/images/2024-10-08-21-41-26-image.png)

```shell
jshell> URL url = new URL("https://www.baidu.com:8080/login/index.html?name=wuzhongpeng&age=23#tip");
url ==> https://www.baidu.com:8080/login/index.html?name=wuzhongpeng&age=23#tip

jshell> url.  #对象调用URL api
equals(            getAuthority()     getClass()         getContent(        
getDefaultPort()   getFile() #获得文件路径+requestString         
getHost()          getPath()          
getPort()          getProtocol()      getQuery()         getRef()  #获得锚点         
getUserInfo()      hashCode()         notify()           notifyAll()        
openConnection(    openStream()       sameFile(          toExternalForm()   
toString()         toURI() 
```

------



#### UDP | TCP

##### Socket :套接字

- 概述:

![](/home/administrator/.config/marktext/images/2024-10-08-22-33-22-image.png)

Socket 编程也叫  TCP编程    或    UDP编程

![](/home/administrator/.config/marktext/images/2024-10-08-22-38-30-image.png)

---

HTTP <--- socket ---> TCP/UDP

- socket 是抽象的，将系统底层的进程通信封装成socket接口,使用现成的接口进行网络编程

![](/home/administrator/.config/marktext/images/2024-10-08-22-32-18-image.png)





--------



#### TCP编程

##### 三次握手（打开通道）

![](/home/administrator/.config/marktext/images/2024-10-09-12-14-38-image.png)

##### 四次挥手（关闭通道）

![](/home/administrator/.config/marktext/images/2024-10-09-12-14-14-image.png)







##### TCP编程概念

- 使用到 socket接口: 支持双向通信： 

三部分： 连接、通信、关闭释放

![](/home/administrator/.config/marktext/images/2024-10-09-12-27-35-image.png)

> - server： 接收请求
> 
> - client : 发送请求
> 
> revc : InputStream  和 send : OutputStream 使用 IO流通信: 实现request \ response...
> 
> - Socket : 也是资源，用完要关闭.close();
> 
> - Socket sck = ....;  sck.outputStream...; sck.inputStream...

 



###### Socket类概述 ：    （客户端要new的类）

![](/home/administrator/.config/marktext/images/2024-10-09-12-42-16-image.png)

> - 先关闭内部 Input/output流；才能关闭socket流





###### ServerSocket类概述：（服务端要new的类）

![](/home/administrator/.config/marktext/images/2024-10-09-12-49-03-image.png)

- ServerSocket(portNumber); //监听： 开放portNumber 给其他机子访问...

- new出来的ServerSocket实例，使用accept(); //服务机“卡住”等待客户机发送请求:  

> ServerSocket server = new ServerSocket(port);
> 
> Socket toClient = server.accept();  //这个对象用于向客户机：getOutputStream() //发消息,还有 toClient.getInputStream();





###### TCP实现-单向数据通信

- Clinet:

```java
package com.wuzhongpeng.network;

import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

public class TCPClient {
    public static void main(String[] args) throws UnknownHostException {
        InetAddress ip = InetAddress.getLocalHost();//获取本地ip地址
        int port = 8888;

        try(Socket client = new Socket(ip,port);//这一步，绑定了远程服务器，连接完成，可进行通信
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(client.getOutputStream()))){ //ip：远程服务器的ip地址：由于是本机服务，所以使用了 localhost...  socket client = new socket(ip,,port); 连接到等待（accept）中的服务器
            //客户端发送请求数据
            bw.write("hello im client end ~~~\n");
            bw.write("hello im client end ~~~\n");
            bw.write("hello im client end ~~~\n");
            System.out.println("send success!");
            bw.flush();

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```



- server：

```java
import java.io.*;
import java.net.*;
public class TCPSocketServer{
    public static void main(String[] args){
            int port = 8888;
            ServerSocket serverSocket = null;
            BufferedReader br = null;
            Socket clientSocket = null; 
            try{
                serverSocket = new ServerSocket(port);
                System.out.println("server start ...");
                System.out.println("server started success listen is "+port+" ...");

                clientSocket =  serverSocket.accept();

                br = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
                String str = null;
                while((str = br.readLine())!=null){
                    System.out.println(str);
                }}catch(Exception e){
                    System.out.println(e.getMessage());
                }finally{
                    if(clientSocket != null){
                       try{
                            clientSocket.close();
                       }catch(Exception e){}
                    }
                    if(br != null){
                       try{
                            br.close();
                       }catch(Exception e){}
                    }
                    if(serverSocket != null){
                       try{
                            serverSocket.close();
                       }catch(Exception e){}
               }
            }

        }
  } 


```



> 先启动 服务器(先监听端口) ；然后 客户端才能发送请求...





###### client ---- server : 循环发消息

```java
//在客户端原有基础上 加 while(true)
            Scanner scanner = new Scanner(System.in);
            while (true) {
                System.out.print("client send message :");
                String mgs = scanner.nextLine();//输入回车符号，照样接收
                bw.write(mgs);
                bw.flush();
                bw.write("\n");
            }

```

// 服务端无需改动:

> 当前实现： 客户端可多次数发送数据，服务器连接状态能一直读到客户端数据～～～
> 
> - 缺陷不足： 客户端停止程序，服务器也停止了...



###### TCP实现-双向数据通信

- 客户机给服务机发送数据

- 服务机 作出 回复 

> 想实现 双向，
> 
> - 也就是，客户端 outputStream,服务端 inputStream;
> 
> - 服务端 OutputStream , 客户端： InputStream



- server-end:

```java
package com.wuzhongpeng.network;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPDuplexServer {
    public static void main(String[] args) {
        //server-end...
        ServerSocket serverSocket = null;
        Socket clientSocket = null;
        int port  = 8888;
        String fileName = "./clienSendImage.jpg";
        BufferedOutputStream bos = null;
        BufferedInputStream bis = null;
        BufferedWriter  bw = null;
        try{
            serverSocket = new ServerSocket(port);
            System.out.println("服务器已启动，监听端口："+ port + " ...");
            clientSocket = serverSocket.accept();

            //服务端接收图片：
            bis = new BufferedInputStream(clientSocket.getInputStream());
            //将文件读入到指定位置
            bos = new BufferedOutputStream(new FileOutputStream(fileName));
            byte[] buffer = new byte[1024];
            int readConut = 0;
            while((readConut = bis.read(buffer))!= -1){
                bos.write(buffer,0,readConut);//将图片流写入到指定文件；
            }
            bos.flush();
            bw = new BufferedWriter(new OutputStreamWriter(clientSocket.getOutputStream()));
            bw.write("server: 收到！");
        }catch(Exception e){
            System.out.println(e.getMessage());
        }finally {
            if (bw != null) {
                try{
                    bw.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
            if (bos != null) {
                try{
                    bos.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
            if (bis != null) {
                try{
                    bis.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
        }
    }
}

```

- client-end:

```java
package com.wuzhongpeng.network;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

public class TCPDuplexClient {
    public static void main(String[] args) throws UnknownHostException {
        int port = 8888;
        String imageName = "/home/administrator/Images/截图/111.jpg";
        Socket client = null;
        BufferedInputStream bis = null;
        BufferedOutputStream  bos = null;
        BufferedReader br = null;
        InetAddress remoteIP = InetAddress.getLocalHost();
        try {
            client = new Socket(remoteIP,port);
            //客户端发送： getOutputStream()
            bis = new BufferedInputStream(new FileInputStream(imageName));
            bos = new BufferedOutputStream(client.getOutputStream());//发送流

            byte[] buffer = new byte[1024];
            int readConut = 0;
            while((readConut = bis.read(buffer)) != -1){
                bos.write(buffer,0,readConut);
            }
            bos.flush();//关于 OutStream 都记得要flush
            client.shutdownOutput();//关闭该次发送数据

            //客户端接收服务器的响应；client.getInputStream();
            br = new BufferedReader(new InputStreamReader(client.getInputStream()));
            String str = null;
            System.out.print("服务器发回消息：");
            while((str = br.readLine())!=null){
                System.out.println(str);
            }

        } catch (IOException e) {
            throw new RuntimeException(e);
        }finally {
            if (br != null) {
                try{
                    br.close();
                }catch(Exception e){
                    System.out.println(e.getMessage());
                }
            }
            if (bos != null) {
                try{
                    bos.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
            if (bis != null) {
                try{
                    bis.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
            if (client != null) {
                try{
                    client.close();
                }catch (Exception e){
                    System.out.println(e.getMessage());
                }
            }
        }
    }
}

```



---



    

#### UDP编程



##### UDP编程概念



- (用户)数据报Socket：

![](/home/administrator/.config/marktext/images/2024-10-09-21-24-44-image.png)



- 数据报`包：

![](/home/administrator/.config/marktext/images/2024-10-09-21-26-32-image.png)





###### UDP编程实例：

- send-end：

![](/home/administrator/.config/marktext/images/2024-10-09-21-32-23-image.png)

- receive-end：

![](/home/administrator/.config/marktext/images/2024-10-09-21-32-39-image.png)
