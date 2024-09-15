# JAVA-集合工具类

> java将数据结构 实现了的工具
> 
> - 在 java.util. 包内**
>   
>   - import java.util.*;
> 
> - 知道什么场景用什么数据结构才是最重要的     

![](/home/administrator/.config/marktext/images/2024-08-27-00-54-31-image.png)

## 集合（Collection）: 单个方式存储

> - 有序集合：有下标索引 or 按大小顺序存取 (满足其一即可是有序队列)
>   
>   - 无序集合：无下标 & 随意存取

![](/home/administrator/.config/marktext/images/2024-08-27-18-50-42-image.png)

- 接口继承关系
  
  ![](/home/administrator/.config/marktext/images/2024-08-27-19-15-57-image.png) 
  
  > ![](/home/administrator/.config/marktext/images/2024-08-27-22-37-54-image.png)

###### collection的类方法：

![](/home/administrator/.config/marktext/images/2024-08-27-23-13-13-image.png)

> contains(); //底层是【比较值】，a.equals(b),值相同就是"contains"

###### 迭代器：

> 父亲是collection的实现类都可使用该迭代器（Iterator: java.util.Iterator）

- 实现原理：
  
  ![](/home/administrator/.config/marktext/images/2024-08-27-23-22-21-image.png)
  
  > 3步骤：
  > 
  > ![](/home/administrator/.config/marktext/images/2024-08-27-23-34-27-image.png)
  
  ---

###### fail-fast机制（“快速失败机制”）

![](/home/administrator/.config/marktext/images/2024-09-03-19-24-24-image.png)

> 图1：指针是"it"

![](/home/administrator/.config/marktext/images/2024-09-03-19-25-03-image.png)

> 图2： 使用names（names是集合）会将元素删除，导致 it 访问不到

- 迭代器CURD  &  集合CRUD会产生异常
  
  - modCount  != exceptedModCount ===> throw new (并发异常)
  
  - 使用 迭代器操作会exceptedModCount =  modCount; 当两者相等没问题
  
  - ```java
    while(it.hasNext()){
        String s = it.next();
        if("bnana".equals(s)){
        list.remove(s); //modCount ++; exceptedModCount(不变)
        //it.remove();//modCount ++  and  exceptedModConut ++
     }
    ```
    
    > ![](/home/administrator/.config/marktext/images/2024-09-03-22-14-40-image.png) : 执行完.next();光标就往下“指针”移动一位

### SequencedCollection

 ![](/home/administrator/.config/marktext/images/2024-08-27-23-46-00-image.png)

#### List

- list具有的方法：![](/home/administrator/.config/marktext/images/2024-09-03-19-51-10-image.png)

##### list【特有】方法：![](/home/administrator/.config/marktext/images/2024-09-03-19-53-39-image.png)

###### .of(...) //<mark>只可读集合</mark>: static(类方法)：

```java
List<Integer> readOnly = List.of(343,545,434,3545,65,7,65);
//readOnly.set(3,545); //error...不可修改...
```

###### List特有方法： <mark>sort()</mark>

> *结合<u>比较器(**comparator**)</u>灵活实现排序：*
> 
> **前面学习**到的 **<u><mark>Arrays.sort(</mark>elements[]<mark>);</mark> </u>**//只能对基本数据类型以及它的对应包装类可实现排序，例如自定义类不可以
> 
> - Arrays.sort();想实现类对象的排序，是使用的compar**able**<T> 接口
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-15-27-54-image.png)
>   
>   - 重写
>     
>     ![](/home/administrator/.config/marktext/images/2024-09-10-15-32-08-image.png)
>     
>     - 如果想按名字排(使用String的equals方法)，那就是：<mark>return this.name.compareTo(user.name);</mark>![](/home/administrator/.config/marktext/images/2024-09-10-22-00-06-image.png)
>   
>   - > compareTo( ) : 返回值 ：（    0：a == b; 
>     > 
>     >                                                    x > 0:  a > b;       
>     > 
>     >                                                     x<0 :  a < b;
>     > 
>     >                                             ）
>     
>     ---
> 
> (方法一)使用 comparable<T> 接口实现的代码：
> 
> user.java
> 
> ```java
>      package com.testList;                                                                                                                                       
>    1 public class user implements Comparable<user>{
>    2 
> -  3     private int age;
> |  4     private String name;
> |  5 
> |  6     public void setName(String name){
> -  7         this.name = name;
> |  8     }
> |  9     public String getName(){
> - 10         return this.name;
> | 11     }
> | 12 
> | 13     public user(int age,String name){
> - 14         this.age = age;
> 2 15         this.name = name;
> | 16     }
> | 17 
> | 18     @Override
> | 19     public String toString(){
> - 20         return "{name : " + this.name + " , age : " + this.age +  "}";
> | 21     }
> |  7     @Override
> |  6     public int compareTo(user u){
> -  5         //return u.age - this.age; //desc
> 2  4         // name sort asc
> 2  3         return this.name.compareTo(u.name);
> |  2     }
>    1 
>  31  }                 
> ```
> 
> UserList.java:
> 
> ```java
>    9 package com.testList;
>    8 import java.util.*;
>    7 import com.testList.user;
>    6 public class UserList{
> -  5     public static void main(String[] args){
> -  4         List<user> list = new ArrayList<>();
> 2  3         list.add(new user(42,"lishi"));
> 2  2         list.add(new user(23,"wuzhongpeng"));
> 2  1         list.add(new user(24,"lining"));
> 210                                                                                                                                                              
> 2  1         Iterator<user> it = list.iterator();
> 2  2         while(it.hasNext()){
> -  3             System.out.println(it.next().toString());
> 2  4         }
> |  5     }
>    6         
>    7 } 
> ```
> 
> - 使用comparable接口实现是在类内部进行添加，不好...<mark>是缺点</mark>：会破坏<mark>opc原则</mark>
> 
>         |
> 
>         V
> 
>     解决办法
> 
>         |
> 
>         V
> 
> <mark>so: 自己写一个比较规则! ******</mark>
> 
> ---
> 
> （方法二）做法：使用compara**<mark>tor</mark>**接口，实现这个接口==自定义一个比较规则！
> 
> ![](/home/administrator/.config/marktext/images/2024-09-10-14-48-49-image.png)
> 
> 用法：
> 
> - (1) 需要排序类
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-22-31-51-image.png)
> 
> - (2) 调用类
>   
>   List的sort方法放入比较器的实例...
>   
>   List<type> list ...
>   
>   <mark>list.sort(new comparator-class-name);</mark>
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-22-35-23-image.png)
> 
> - (3) 实现compartor排序器to类(<mark>负责写比较规则</mark>)
>   
>   list.sort(接收一个比较规则的实例，sort就可以根据规则进行asc/desc...)
>   
>   > 重写 `compare`方法，`compare`方法是comparator接口规定的抽象方法
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-22-33-13-image.png)
>   
>   ---
>   
>   （方法三) 使用匿名内部类：
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-11-15-14-05-image.png)
>   
>   也就是将：![](/home/administrator/.config/marktext/images/2024-09-11-15-14-52-image.png)，改成匿名类的形式，优点就是，少写一个 comparetor接口的实现类
>   
>   【*】源代码：
>   
>   - User.java
>   
>   ```java
>   package com.comparator.test;
>   
>   public class User{
>       private String name;
>       private int age;
>   
>       public User(String name, int age){
>           this.name = name;
>           this.age = age;
>       }
>   
>       public User(){}
>   
>       public void setName(String name){
>           this.name = name;
>       }
>       public String getName(){
>           return this.name;
>       }
>       public void setAge(int age){
>           this.age = age;
>       }
>       public int getAge(){
>           return this.age;
>       }
>   
>       @Override
>       public String toString(){
>           return "{ " + this.name + " , " + this.age + " }"; 
>       }
>   
>   }
>   ```
>   
>   - UserListMain.java
>   
>   ```java
>   package com.comparator.test;
>   
>   import java.util.*;
>   import com.comparator.test.User;
>   public class UserListMain{
>       public static void main(String[] args){
>           List<User> list = new ArrayList<>();        
>           list.add(new User("zhanshan",28));
>           list.add(new User("lishi",43));
>           list.add(new User("wangwu",17));
>           list.add(new User("zhaoli",32));
>           list.add(new User("benzhu",66));
>   
>           /* 实现comparator比较器接口的方式
>               list.sort(new UserComparator());
>           */
>   
>           /* 使用匿名类的方式 */
>           list.sort(new Comparator<User>(){
>               @Override
>               public int compare(User u1, User u2){
>                   return Integer.compare(u1.getAge(),u2.getAge());//u1.getAge() - u2.getAge()  //Order : asc  
>                    }
>                }
>             );
>   
>           Iterator<User> it = list.iterator();
>           while(it.hasNext()){
>               User user = it.next();
>               System.out.println(user.toString());
>           }
>   
>       }
>   
>   }
>   ```
>   
>   - UserComparator.java
>   
>   ```java
>   package com.comparator.test;
>   import com.comparator.test.User;
>   import java.util.*;
>   public class UserComparator implements Comparator<User>{
>       @Override
>       public int compare(User u1, User u2){
>           return Integer.compare(u2.getAge(),u1.getAge());  //order : desc
>       }
>   }
>   ```

---

###### List类 <mark>特有的 迭代器： ListIterator<></mark>     .listIterator()

> iterator() 集合都可遍历
> 
> ![](/home/administrator/.config/marktext/images/2024-09-03-22-01-51-image.png)
> 
> listIterator() 只有list可遍历
> 
> ![](/home/administrator/.config/marktext/images/2024-09-03-22-02-15-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-03-22-07-20-image.png)
> 
> - <mark>特殊(1)：set()</mark>
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-03-22-20-29-image.png)
>   
>   li.set("XXX") ===> <u>修改的是 ： .next()；获取到的那个数</u>： “name” = "XXX"
> 
> - <mark>特殊(2): remove();</mark>
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-03-22-23-42-image.png)

> - remove()  : 删除的是 .next()获取的那个对象

- 

- > - next() : 取出“指针”当前值，并“指针”向下移动一格...
  > 
  > - pervious() : 指针向上移动一格，然后取值

- perviousIndex()  :只获取，不移动...
  
  ![](/home/administrator/.config/marktext/images/2024-09-09-20-10-15-image.png)

- nextIndex()  ： 只获取下标位置，不移动...
  
  > 获取当前“指针”的下标（从0开始）

#### ArrayList

- size: 数据结构内部存入的元素个数

- length: 数组数据结构的空间大小(个数)

###### ArrayList的构造器(3个)：

![](/home/administrator/.config/marktext/images/2024-09-11-19-46-09-image.png)

- 无参： List<Integer> listInt = new ArrayList<>();

- 有参1：List<String> listStr = new ArrayList<>(1500); //预估，可避免多次扩容影响性能

- 有参2： List<Integer> listarr = new ArrayList<>(Arrays.asList( arr ));

###### 概念：

![](/home/administrator/.config/marktext/images/2024-09-11-19-32-43-image.png)

---

###### .add(); //添加

- 新版jdk()，new的时候(调用无参数构造函数时...)为<mark> 0 </mark>的长度【老版本是10个长度】
  
  ```java
  List<TYPE> list = new ArrayList<>();
  ```

- 当开始(第一次)添加`.add()`时，数组长度置为 <mark>10 </mark>；
  
  > ![](/home/administrator/.config/marktext/images/2024-09-11-19-03-22-image.png)
  
  ```java
  list.add("xxx");
  list.add("yyy");
  ...
  ```

- 当add 大于等于 数组长度，就触发"扩容"
  
  ![](/home/administrator/.config/marktext/images/2024-09-11-19-06-07-image.png)
  
  > 执行了grow(),扩容函数...
  > 
  > ![](/home/administrator/.config/marktext/images/2024-09-11-19-07-24-image.png)
  > 
  > ![](/home/administrator/.config/marktext/images/2024-09-11-19-08-07-image.png)
  > 
  > int prefLength = oldLength + Math.max(...);  //结论，每次扩容能扩至原本的<mark> 1.5 </mark>倍；如oldList => 10;  newList => (10 * 1.5) =15

###### .set(); //修改

![](/home/administrator/.config/marktext/images/2024-09-11-19-59-21-image.png)

```java
list.set(index-number,elemnet-value);
/**
*  @return 返回没修改前的index下标位置的 元素值
*/
```

###### .add(index-number,insert-value); //插入

```java
list: [1,2,3,4,5,6,7,8]
list.add(2,666);
//list: [1,2,666,4,5,6,7,8]


List<String> listName = new ArrayList<>();
listName.add("one");
listName.add("two");
listName.add("there");

list.add(2,"TWO");

//listName : one two TWO 
```



###### remove(index);

![](/home/administrator/.config/marktext/images/2024-09-13-21-58-43-image.png)

into:

![](/home/administrator/.config/marktext/images/2024-09-13-21-57-55-image.png)

> 删除需要移动数组，使用System.arraycopy()；可以提高效率(底层使用c++)：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-13-22-00-23-image.png)
> 
> - 效率要比使用java写的Arrays.copyOf()效率要高...



#### LinkedList

 

## Map(**) ：两个方式存储

### HashMap

### TreeMap

## Collections-工具类
