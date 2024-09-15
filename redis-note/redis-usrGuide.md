## redis

![](/home/administrator/.config/marktext/images/2024-09-07-16-01-39-image.png)

### 数据类型：

五种基本数据类型                                                        五种高级数据类型      

![](/home/administrator/.config/marktext/images/2024-09-06-21-07-06-image.png)

#### 使用方式（3种）：

     ![](/home/administrator/.config/marktext/images/2024-09-06-21-08-26-image.png)

> - CLI : 命令行方式（同mysql一样）
> 
> - API：应用程序接口（编程语言调用）
> 
> - GUI：...图形化操作界面

#### 特点：

![](/home/administrator/.config/marktext/images/2024-09-06-21-11-00-image.png)

### 下载

```shell
sudo apt install redis
```

#### 启动

```shell
systemctl start redis-server
```

#### 测试连接情况

```shell
redis-cli -p 端口号 ping  //回复‘PONG’表示启动成功
```

- redis可通过配置文件修改端口号：
  
  ```shell
  sudo vim  /etc/redis/redis.conf
  ```
  
  修改：
  
  ![](/home/administrator/.config/marktext/images/2024-09-06-21-52-43-image.png)

重启：

```shell
sudo systemctl restart redis
```

----

### 使用redis

>  先开redis服务;后开客户端
> 
> redis-server
> 
> redis-cli
> 
> ![](/home/administrator/.config/marktext/images/2024-09-06-21-58-02-image.png)

### redis操作

> redis 是以key-value 的存储方式

#### (1)：String(默认存储，任何类型：bool，int，...)

##### set：(“做”出一个键值对)

```shell
SET name "zhangshan"
set name_2 lishi
```

![](/home/administrator/.config/marktext/images/2024-09-07-12-34-19-image.png)

##### get:(get键，获取值)

```shell
get name
get name_2
```

![](/home/administrator/.config/marktext/images/2024-09-07-12-32-03-image.png)

##### del:(删除)

![](/home/administrator/.config/marktext/images/2024-09-07-12-36-26-image.png)

##### exists:(判断存在？)

![](/home/administrator/.config/marktext/images/2024-09-07-12-37-49-image.png)

##### keys:(列出对应key的值)

![](/home/administrator/.config/marktext/images/2024-09-07-12-39-54-image.png)

- keys * ：列出全部key

支持正则表达：

![](/home/administrator/.config/marktext/images/2024-09-07-12-41-16-image.png)

##### flushall:(清除全部key-value)

![](/home/administrator/.config/marktext/images/2024-09-07-12-42-12-image.png)

使用 --raw  选项参数可设置原始结果（默认以二进制，所以中文文本会变成进制表示），显示文本中文

![](/home/administrator/.config/marktext/images/2024-09-07-12-45-39-image.png)

##### ttl：（查看过期时间）

![](/home/administrator/.config/marktext/images/2024-09-07-12-48-03-image.png)

> -1 表示没有设置过期时间

##### expire:(设置过期时间)

![](/home/administrator/.config/marktext/images/2024-09-07-12-49-51-image.png)

> 设置10秒，时间到，key-value 被释放

##### setex:(设置可过期key-value)

![](/home/administrator/.config/marktext/images/2024-09-07-12-56-53-image.png)

##### setnx:(当键不存在是”set“，存在就不创建)

![](/home/administrator/.config/marktext/images/2024-09-07-13-00-07-image.png)

#### (2)：List

##### Lpush/Rpush

- Lpush(<mark>头插入</mark>)
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-01-00-image.png)
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-01-38-image.png)
  
  继续：![](/home/administrator/.config/marktext/images/2024-09-07-14-07-38-image.png)
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-07-49-image.png)
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-08-13-image.png)

    继续添加：![](/home/administrator/.config/marktext/images/2024-09-07-14-09-30-image.png)

               Lpush letter c d e == Lpush letter c; Lpush letter d; Lpush letter e

    ![](/home/administrator/.config/marktext/images/2024-09-07-14-11-11-image.png)

- 可一次添加多个元素：
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-28-09-image.png)

- 同理： 使用Rpush：尾添加

##### Lrange `<letter:arr_name>` <index:(0):head>  <index:(-1):end>

> 0 -1 ：表示从首地址开始range到最后一个数组元素

![](/home/administrator/.config/marktext/images/2024-09-07-14-05-44-image.png)

- 同理还有Range

##### 删除：pop（Rpop/Lpop）

- Rpop <list—name>：删除列表最后一个元素

- Lpop <list—name>：删除列表第一个元素

![](/home/administrator/.config/marktext/images/2024-09-07-14-13-53-image.png)

- 也可一次删除多个（老版本不支持）：请更新到6.2以上
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-14-18-32-image.png)   

##### Llen: 获取当前list的长度

![](/home/administrator/.config/marktext/images/2024-09-07-14-22-48-image.png)

##### RpopLpush消息队列：先进先出

![](/home/administrator/.config/marktext/images/2024-09-07-14-24-57-image.png)

> R:pop; L:push;

##### ltrim：（截断，保留范围内的元素：从0下标开始）

![](/home/administrator/.config/marktext/images/2024-09-07-14-32-37-image.png)

---

#### (3)：Set（不可添加重复元素的集合）

<mark>命令特点：以“**s**” 开头</mark>

- set相关命令都是以S开头

##### sadd：添加集合元素(可以一次添加多个set成员...)

  ![](/home/administrator/.config/marktext/images/2024-09-07-15-21-23-image.png)

##### smembers:查看set元素列表：

![](/home/administrator/.config/marktext/images/2024-09-07-15-23-39-image.png)

##### sismember:判断元素是否存在set中

![](/home/administrator/.config/marktext/images/2024-09-07-15-26-38-image.png)

> 0:不存在
> 
> 1：存在

##### srem:删除set元素

```shell
srem <set-name> <set-element>
```

![](/home/administrator/.config/marktext/images/2024-09-07-15-29-31-image.png)

##### 集合运算：

![](/home/administrator/.config/marktext/images/2024-09-07-15-31-16-image.png)

> 交  、并、差
> 
> ![](/home/administrator/.config/marktext/images/2024-09-07-15-36-36-image.png)

#### (4)：SortedSet:有序集合

![](/home/administrator/.config/marktext/images/2024-09-07-15-45-21-image.png)

<mark>命令特点：以“**Z**” 开头</mark>

##### zadd:有序集合添加元素

```shell
zadd <sortedSet-name> [member1] [member2] ...
```

![](/home/administrator/.config/marktext/images/2024-09-07-15-47-15-image.png)

##### zrange 列出有序集合成员：

z     range

```shell
zrange <sortedSet-name> <start-index> <end-index>
```

![](/home/administrator/.config/marktext/images/2024-09-07-15-48-18-image.png)

- 输出成员and分数：+      ` withscores`
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-15-50-46-image.png)

##### zscore : 查看成员分数（只查看分数不看成员）

z    score

```shell
zscore <sortedSet-name> <member-name>
```

![](/home/administrator/.config/marktext/images/2024-09-07-15-53-01-image.png)

##### zrank:查看成员排名:（从小到大）

```shell
zrank <sortedSet-name> <member-name>
```

![](/home/administrator/.config/marktext/images/2024-09-07-15-56-20-image.png)

> 从0开始：”2“ 表示”haikou“ 在 下标索引的 2，也就是第三个 

##### zrevrank: 从大到小查看成员位置

- z    rev    rank

![](/home/administrator/.config/marktext/images/2024-09-07-16-00-31-image.png)

#### (5)：Hash:哈希键值对

![](/home/administrator/.config/marktext/images/2024-09-07-16-01-21-image.png)

命令特点：以 “H”开头

##### Hset：添加键值对

```shell
hset <hash-table-name> <key> <value>
```

![](/home/administrator/.config/marktext/images/2024-09-07-16-08-10-image.png)

##### Hget:获取hash成员

```shell
hget <hashtable-name> <hash-key>
```

![](/home/administrator/.config/marktext/images/2024-09-07-16-09-37-image.png)

- hgetall:获取hash全部成员信息（key-value）
  
  ![](/home/administrator/.config/marktext/images/2024-09-07-16-11-53-image.png)

##### hdel:删除键值对

![](/home/administrator/.config/marktext/images/2024-09-07-16-16-09-image.png)

##### hexists: 判断键值对是否存在

![](/home/administrator/.config/marktext/images/2024-09-07-16-18-24-image.png)

> 0:false
> 
> 1:true

##### hkeys:获取hash所有键

![](/home/administrator/.config/marktext/images/2024-09-07-16-20-05-image.png)

##### hlen:<mark>返回</mark>hashtable的key-value<mark>个数</mark>

![](/home/administrator/.config/marktext/images/2024-09-07-16-22-00-image.png)

### (6)：发布订阅模式

- 发布：publish

- 订阅：subscribe

###### 使用：

1.设置<mark>发布者：（使用publish）</mark>

```shell
publish <频道名> <发布的信息>
```

![](/home/administrator/.config/marktext/images/2024-09-08-15-08-20-image.png)

> 0:没有订阅
> 
> 2：有两个订阅
> 
> 3： 有3个订阅者

2. <mark>订阅者使用： subscribe</mark>

```shell
subscribe <接收哪个频道>
```

![](/home/administrator/.config/marktext/images/2024-09-08-15-10-07-image.png)

###### 实验结果：

> 可有多个订阅者订阅，发布者发布消息，已订阅的都可收到消息...

![](/home/administrator/.config/marktext/images/2024-09-08-15-13-08-image.png)

###### <mark>缺点</mark>：发布的消息messages<mark>无法持久化</mark>

> **使用 stream ：消息队列 可解决持久化问题！**

---

### (7)：消息队列：Stream

> - redis -v 3.5 推出 stream
> 
> - Stream 相关命令使用 X 开头

##### xadd:添加消息

```shell
xadd <key:消息队列的名称> <id:可默认‘ * ‘，或手动设置> <属性> <属性值> 
```

![](/home/administrator/.config/marktext/images/2024-09-08-20-08-22-image.png)

> 可手动设置 id ：
> 
> xadd key-name  133(时间戳)-0(次序)  field value
> 
> //   此key的下一个消息id必须大于上一个，不然会报错
> 
> ![](/home/administrator/.config/marktext/images/2024-09-08-20-37-05-image.png)

##### xlen:获取长度（消息个数）

![](/home/administrator/.config/marktext/images/2024-09-08-20-09-33-image.png)

##### xrange:列出消息队列的全部成员

```shell
xrange <key:“发布者名”> < - > < + >
```

![](/home/administrator/.config/marktext/images/2024-09-08-20-10-51-image.png)

##### xdel : 删除队列消息

```shell
xdel <key> <id>
```

![](/home/administrator/.config/marktext/images/2024-09-08-20-13-52-image.png)

> 删除后的消息队列：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-08-20-14-35-image.png)

##### xtrim:裁减

```shell
xtrim <key> MaxLen <保留几个>
```

![](/home/administrator/.config/marktext/images/2024-09-08-20-26-26-image.png)

> 例： xtrim "key" MAXALEN 3
> 
> 表示：保留 3个

--- 以上是消息的生产，如何消费消息？

##### xread

```shell
xread count <个数：n> /*一次读n个*/ [block] [1000]//ms  <streams> <开始读取的位置下标>
//下标从 0 开始
```

![](/home/administrator/.config/marktext/images/2024-09-08-20-44-30-image.png)

![](/home/administrator/.config/marktext/images/2024-09-08-20-45-15-image.png)

- 使用 block <毫秒数> :当读取的位置为空时，堵塞(时长：根据设置的毫秒数)，当堵塞时间到达时，还无消息，那就输出 null

- 使用 $ 可获取 "此后"的消息
  
  (消费者)等待读取最新消息：
  
  ![](/home/administrator/.config/marktext/images/2024-09-08-20-56-27-image.png)

       (生产者)产生新消息：

       ![](/home/administrator/.config/marktext/images/2024-09-08-20-58-14-image.png)      

       (result):

       ![](/home/administrator/.config/marktext/images/2024-09-08-20-59-05-image.png)

- 消费者可设置消费者组（消费者组放入多个消费者）：
  
  ##### xgroup
  
  ```shell
  xgroup create <消息队列名> <设置组名> <设置组id>
  ```
  
  ![](/home/administrator/.config/marktext/images/2024-09-08-21-10-02-image.png)
  
  ##### xinfo: 显示全部消息组信息数据...
  
  ```shell
  xinfo groups <消息队列名> 
  ```
  
  ##### xgroup createconsumer : 加入消费者
  
  ![](/home/administrator/.config/marktext/images/2024-09-08-21-22-01-image.png)

##### xreadgroup: 读取消息组中消息成员的消息

![](/home/administrator/.config/marktext/images/2024-09-08-21-28-53-image.png)

### 

### 地理空间：

- geo开头

##### geoadd:添加地理坐标

...

### 事务处理



### 持久化

![](/home/administrator/.config/marktext/images/2024-09-09-12-57-48-image.png)

#### RDB：(/etc/redis/redis.conf)

![](/home/administrator/.config/marktext/images/2024-09-09-12-59-00-image.png)

> save <秒数>  <次数>    
> 
> 例如：` save 900 1 ` ： 表示 900秒内有一次修改就保存数据到（<mark>快照</mark>）磁盘

- 也可手动在redis-cli内手动保存：命令行内输入     `save`  ； 
  
  ![](/home/administrator/.config/marktext/images/2024-09-09-13-02-08-image.png)
  
  > 使用 save保存会生成快照文件 `dump.rdb`

- 使用crontab 执行 0点定时任务将保存的快照备份最安全



#### AOF

![](/home/administrator/.config/marktext/images/2024-09-09-13-10-16-image.png)





### 主从redis服务配置



### 哨兵模式
