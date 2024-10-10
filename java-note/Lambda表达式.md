## Lambda表达式 ： OOF -> 面向函数编程

> Lambda表达式 只能 是 接口

?疑问？ 编译器怎么知道，我使用了lambda表达式，对应的是调用了哪个方法？

答：有控制只能使用某种类型； 某类型内有多个方法，那又是怎么知道调用的就是这个？： 使用了<mark> @FuntionInterface </mark>标注(“这个方法能用lambda表达式”)    

- 因为FuntionInterface 注解 规定接口内部只能有一个 可使用lambda的方法，确定了它的<mark>唯一性</mark>

如：    

![](/home/administrator/.config/marktext/images/2024-10-09-22-42-35-image.png)







- 补充： 匿名内部类new之后，会生成一个xxx$111.class 文件；lambda表达式不会    



##### lambda表达式和 匿名内部类的区别：

![](/home/administrator/.config/marktext/images/2024-10-10-19-29-50-image.png)





#### lambda 表达式的使用：

- 无返回值无参数

![](/home/administrator/.config/marktext/images/2024-10-10-19-56-41-image.png)

> NoParameteerNoReturn npnr2 = () -> System.out.println("xxxxxx");

- 无返回值有参数

![](/home/administrator/.config/marktext/images/2024-10-10-19-57-38-image.png)

> OneParamterNoReutrn opnr = value -> System.out.println("Integer ---> " + values);

![](/home/administrator/.config/marktext/images/2024-10-10-19-58-49-image.png)

> MoreParameterNoReturn mpnr  = (value1,value2) -> System.out.println(value1 + value2);

---

- 有返回值 无参数

![](/home/administrator/.config/marktext/images/2024-10-10-20-00-24-image.png)

> NoParameterHasReturn nphr = () -> 500;

- 有返回值有参数

![](/home/administrator/.config/marktext/images/2024-10-10-20-01-34-image.png)

> OneParameterHasRuturn ophr = value -> value * 2'

- 有返回值，多个参数

![](/home/administrator/.config/marktext/images/2024-10-10-20-03-05-image.png)



###### >lambda表达式还可以更精简：

    ![](/home/administrator/.config/marktext/images/2024-10-10-20-21-07-image.png)





#### 迎合Lambda表达式的函数接口：基本的，它的衍生子类很多个的；

> 遇到这四个相关(单词包含，即可判断有相关)的，都可联想到要使用 lambda表达式...

![](/home/administrator/.config/marktext/images/2024-10-10-20-29-05-image.png)

![](/home/administrator/.config/marktext/images/2024-10-10-20-29-31-image.png)







#### 方法引用：简化lambda表达式

<mark>！！！！！！ “参数类型一样，返回值类型一样”！！！！！！</mark>

##### 实例方法 引用

![](/home/administrator/.config/marktext/images/2024-10-10-20-36-54-image.png)

> User :: getAge  ;<mark> 对象名：：实例方法名</mark>

![](/home/administrator/.config/marktext/images/2024-10-10-20-43-09-image.png)

> 函数接口函数 返回值 and 接口函数参数   与   该函数内部的 返回值and参数 一样！

- 实现：

![](/home/administrator/.config/marktext/images/2024-10-10-20-45-41-image.png)

-再来一个：

![](/home/administrator/.config/marktext/images/2024-10-10-20-50-17-image.png)

> <mark>对象名：：实例方法名</mark>
> 
> - System.out 是对象， printStream类的实例对象

###### 小技巧： lambda表达式 参数 和 方法参数一样，几乎都可以使用简化

![](/home/administrator/.config/marktext/images/2024-10-10-20-54-02-image.png)

> System.out :: Println

![](/home/administrator/.config/marktext/images/2024-10-10-20-54-52-image.png)

> tracher::getName

##### 静态方法 引用

![](/home/administrator/.config/marktext/images/2024-10-10-20-57-25-image.png)

> 还是那个技巧： lambda的参数 和 静态方法传入的参数一样，so...
> 
> <mark>Math(类名) ：：round("静态方法名")</mark>



##### 特殊方法 引用

![](/home/administrator/.config/marktext/images/2024-10-10-20-59-19-image.png)

> <mark>类名 ：： 实例方法名</mark>
> 
> - 是 实例方法引用 + 静态方法引用的 混合...

- 理解： 第一个参数当调用方法的对象，第二个参数当方法的参数:<mark> 返回值也要相同</mark>

> ![](/home/administrator/.config/marktext/images/2024-10-10-21-03-43-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-10-10-21-05-31-image.png)
> 
> (o1,o2) -> o1.compareTo(o2) 
> 
> +++++++++++++++++++++++
> 
> Double :: compareTo   //o1是Double类型
> 
> ![](/home/administrator/.config/marktext/images/2024-10-10-21-08-12-image.png)

##### 构造方法 引用

> <mark>类名 ：：new</mark>

      ![](/home/administrator/.config/marktext/images/2024-10-10-21-12-28-image.png)

> 内部进行 了 new 构造函数...

![](/home/administrator/.config/marktext/images/2024-10-10-21-14-29-image.png)



![](/home/administrator/.config/marktext/images/2024-10-10-21-15-50-image.png)



##### 数组 引用

> <mark>数组类型 ：：new</mark>
> 
> - 参数是 数组的长度： 是数组[内的“参数”]

![](/home/administrator/.config/marktext/images/2024-10-10-21-18-26-image.png)

> 发现都一个规律： 返回值类型一样、参数一样

![](/home/administrator/.config/marktext/images/2024-10-10-21-21-18-image.png)







###### .forEach( 接口函数类型..)

![](/home/administrator/.config/marktext/images/2024-10-10-21-25-18-image.png)

- 可简化：

```java
list.forEach(System.out::println);
```

![](/home/administrator/.config/marktext/images/2024-10-10-21-27-16-image.png)

- 比 for（：）和 Iterator 好用？？？
  
  -     有这个 forEach()，那Iterator还有使用场景吗

![](/home/administrator/.config/marktext/images/2024-10-10-21-30-40-image.png)

> 使用 forEach确实可以简化map的遍历，比Set<Map.Entry<K,V>> xxx 好用





###### .removeif()

![](/home/administrator/.config/marktext/images/2024-10-10-21-35-29-image.png)
