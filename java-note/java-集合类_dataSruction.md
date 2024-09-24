# JAVA-集合类

- 集合类使用的都是 引用类型

> java将数据结构 实现了的工具
> 
> - 在 java.util. 包内**
>   
>   - import java.util.*;
> 
> - 知道什么场景用什么数据结构才是最重要的     

![](/home/administrator/.config/marktext/images/2024-08-27-00-54-31-image.png)

## Collection: 单个方式存储（图解）

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

---

### Sequenced-Collection

 ![](/home/administrator/.config/marktext/images/2024-08-27-23-46-00-image.png)

---

#### List

- list具有的方法：![](/home/administrator/.config/marktext/images/2024-09-03-19-51-10-image.png)

##### list【特有】方法：![](/home/administrator/.config/marktext/images/2024-09-03-19-53-39-image.png)

###### .of(...) //<mark>只可读集合</mark>: static(类方法)：

```java
List<Integer> readOnly = List.of(343,545,434,3545,65,7,65);
//readOnly.set(3,545); //error...不可修改...
```

###### List特有方法： <mark>sort()</mark>******

> *结合<u>比较器(**<mark>comparator</mark>**)</u>灵活实现排序：*
> 
> **前面学习**到的 **<u><mark>Arrays.sort(</mark>elements[]<mark>);</mark> </u>**//只能对基本数据类型以及它的对应包装类可实现排序，例如自定义类不可以
> 
> - Arrays.sort();想实现类对象的排序，是<mark>使用的compar**able**<____T___> 接口</mark>
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-15-27-54-image.png)
>   
>   - 重写 compareTo()
>     
>     ![](/home/administrator/.config/marktext/images/2024-09-10-15-32-08-image.png)
>     
>     - 如果想按名字排(使用String的equals方法)，那就是：<mark>return this.name.compareTo(user.name);</mark>![](/home/administrator/.config/marktext/images/2024-09-10-22-00-06-image.png)
>   
>   - > compareTo( ) : 返回值 ：（          0：a == b; 
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
> - (1) 待排序类
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-22-31-51-image.png)
> 
> - (2) 调用类
>   
>   List的sort方法放入比较器的实例...
>   
>   List<type> list ...
>   
>   <mark>list.sort(new comparator_class_name);</mark>
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-10-22-35-23-image.png)
> 
> - (3) 实现compartor排序器to类(<mark>负责写比较规则</mark>) <-- 第三个类
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
>           return Integer.compare(u2.getAge(),u1.getAge());  //order : desc、
>                           //  u2.getAge() - u1.getAge()
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

##### ArrayList

- size: 数据结构内部存入的元素个数

- length: 数组数据结构的空间大小(个数)

###### ArrayList的构造器(3个)：

![](/home/administrator/.config/marktext/images/2024-09-11-19-46-09-image.png)

- 无参： List<Integer> listInt = new ArrayList<><mark>();</mark>

- 有参1：List<String> listStr = new ArrayList<><mark>(1500);</mark> //预估，可避免多次扩容影响性能

- 有参2： List<Integer> listarr = new ArrayList<><mark>(Arrays.asList( arr ))</mark>;
  
  > Integer[] arr = new Integer[10];
  > 
  > .........

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
> - 效率要比使用java写的Arrays.<mark>copyOf</mark>()效率要高...
>   
>   >   [][][][][][]   
>   >   | | | | |
>   > 
>   >   V V V V V
>   > 
>   >   [][][][][][][][][][][]

##### Vector

> (是线程安全的，使用<mark>synchronized</mark>) , 早期的集合类，<mark>效率比较低</mark>，<mark>现在很少使用</mark>这个(它的继承类 stack类也一样,很少使用); 现在有新手段实现该形式的线程安全

- Stack（声明 和 使用）和ArrayLlist差不多吧？ 都是属于有序集合的一种...  ！ 底层也是数组...
  
  ```java
  List<String> list = new Vector<>();
  list.add("zhanshan");
  list.add("lishi");
  list.add("wangwu");
  ```
  
  -默认初始化：<mark>10</mark>
  
  ![](/home/administrator/.config/marktext/images/2024-09-17-22-31-53-image.png)
  
  > so? add()..超过10个就要开始扩容了，那Vector的扩容方式是什么？    
  > 
  > ![](/home/administrator/.config/marktext/images/2024-09-17-22-37-52-image.png)
  > 
  > - 结论：<mark>两倍增长</mark>
  >   
  >   ![](/home/administrator/.config/marktext/images/2024-09-17-22-40-17-image.png)

###### stack

> <mark>不常用（因为是基于 Vector 实现的，是线程安全的，慢...)</mark> , FILO ：first int last out 
> 
> - stack 可使用 ：数组 和 链表实现
> 
> - java的Stack使用的是 **数组**实现...   (其实很多场景都是使用的数组,<mark>检索效率高</mark>)
> 
> - **java的LinkedList  可模拟实现 Stack** (底层：链表：linkedllist也是实现了Deque接口)
> 
> - 使用 <mark>deque 接口</mark> 也实现 可stack的（<mark>自定义DIY ，ArrayDeque</mark>）,底层：数组模拟
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-20-21-12-27-image.png)
> 
> >  实现栈的实际类：
> > 
> > - Stack
> > 
> > - ArrayDeque
> > 
> > - LinkedList
> > 
> > 实现队列的实现类：
> > 
> > - ArrayDeque（双端队列）
> > 
> > - LinkedList (双端队列 )

##### LinkedList

> - 双向链表：（不是环形）；
> 
> - linkedlist 初始化是个nul（没有创建结点：`size = 0`）；
> 
> ![](/home/administrator/.config/marktext/images/2024-09-17-23-10-26-image.png)
> 
> - linkedllist的数据结构：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-17-23-01-10-image.png)
> 
> - first ==> 向回指；
> 
> - last ==> 向后指；
>   
>   - Node 结点： 【  previous  | data | next】
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-17-23-08-50-image.png)

- 基本操作：

![](/home/administrator/.config/marktext/images/2024-09-17-23-00-02-image.png)

> add();//默认是append(尾部插入)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-17-23-19-26-image.png)
> 
> - add( index,value);  //选择插入位置：insert-node(插入新结点)
>   
>   - add()是 使用了方法重载，有尾插入的add(value);
> 
> - set(index ,value); //修改链表中结点 ： update-node(修改结点)
> 
> - remove(index); //删除结点
> 
> - get(index) ; //获取指定结点的值(只获取不修改)

##### ArrayDeque(实现栈Stack)

![](/home/administrator/.config/marktext/images/2024-09-20-21-24-38-image.png)

老旧现在不使用了的栈stack：

![](/home/administrator/.config/marktext/images/2024-09-20-21-25-44-image.png)

arrayDeque:

```java
ArrayDeque<Integer> arrDequeStack = new ArrayDeque<>();
arrDequeStack.push(xxx);
arrDequeStack.pop();
```

![](/home/administrator/.config/marktext/images/2024-09-20-21-26-20-image.png)

> LinkedList也有 push 和 pop... 我还以为是要使用add 和 remove模拟呢...ahaha
> 
> ```java
> LinkedList<Integer> linkedList = new LinkedList<>();
> linekdlist.push();
> linkedlist.pop();
> ```

##### ArrayDeque(有双端队列)  --> 使用 Queue接口实现的

- > LinkedList(也可调用实现双端队列)
  > 
  > ![](/home/administrator/.config/marktext/images/2024-09-20-22-12-40-image.png)
  > 
  > ![](/home/administrator/.config/marktext/images/2024-09-20-22-29-24-image.png)
  > 
  > 什么叫双端队列？
  > 
  >  ![](/home/administrator/.config/marktext/images/2024-09-20-22-30-00-image.png)
  > 
  > 答： 两端都可实现 队列（出队如队）
  > - arrayDeque底层是使用<mark>环形数组</mark>实现的（<mark>循环数组</mark>）
  >   
  >   - ![](/home/administrator/.config/marktext/images/2024-09-20-22-47-53-image.png)  
  >   
  >   - ![](/home/administrator/.config/marktext/images/2024-09-20-22-48-26-image.png)
  > 
  >       
  > 
  > ```
  > --[]--[]--[]--[]--[]--[]--[]--[]--[]---
  >   |                                |
  >   V                                V
  >  offer(进)                         poll（出）
  > ```
  > 
  > - LinkedList 也能实现双端队列(双向链表，本就是可往前和往后)

###### ArrayDequ 队列的使用：

```java
Queue<Integer>  queue = new ArrayDeque<>();
queue.offer(1);
queue.offer(2);
...
queue.poll();
queue.poll();
```

###### LinkedList 队列的使用：

```java
Queue<Integer> queue_1 = new LinkedList<>();
queue_1.offer(1);
queue_1.offer(2);
...
```

###### arrayDeque 双端队列使用

```java
import java.util.*;

/*                        first               last
    .offerLast()          head_A ----------> end_B
    .offerFirst()       .offerFirst          .pollLast

    .pollLast()
    .pollFist()

*/

public class ArrayQueue{

    public static void main(String[] args){
        Deque<Integer> q = new ArrayDeque<>();
        q.offerLast(1);//队尾进队
        q.offerLast(2);
        q.offerLast(3);
        System.out.println(q.pollFirst());//队头出队
        System.out.println(q.pollFirst());
        System.out.println(q.pollFirst());

        q.offerFirst(4);//队头进队
        q.offerFirst(5);
        q.offerFirst(6);
        System.out.println(q.pollLast());//队尾出队
        System.out.println(q.pollLast());
        System.out.println(q.pollLast());
    }
}
```

###### linkedList 双端队列使用：

> 用法和 arrayDeque一样

```java
import java.util.*;
public class ArrayQueue{

    public static void main(String[] args){
        Deque<Integer> q = new LinkedList<>();
        System.out.println("LinkedList实现双端队列");
        q.offerLast(1);//队尾进队
        q.offerLast(2);
        q.offerLast(3);
        System.out.println(q.pollFirst());//队头出队
        System.out.println(q.pollFirst());
        System.out.println(q.pollFirst());

        q.offerFirst(4);//队头进队
        q.offerFirst(5);
        q.offerFirst(6);
        System.out.println(q.pollLast());//队尾出队
        System.out.println(q.pollLast());
        System.out.println(q.pollLast());
    }
}
```

## Map(**) ：两个方式存储<K ,V>

- map 和 collection是两个不同的分支

- map的继承关系图：

![](/home/administrator/.config/marktext/images/2024-09-21-13-13-23-image.png)

![](/home/administrator/.config/marktext/images/2024-09-21-13-57-36-image.png)

###### map的体系图解

![](/home/administrator/.config/marktext/images/2024-09-21-14-11-50-image.png)

> collection类的set部分的实现是实现(或继承的 Map的类...),所以，set和map的部分是一一对应的
> 
> - HashSet <--- HashMap
> 
> - LinkedHashSet <-- LinkedHashMap
> 
> - TreeSet <-- TreeMap
> 
> - ....Set <---  .....Map
> 
> collection的set 借用了 map 来实现

#### hash 也叫 ‘散列’

> 底层数据结构：
> 
> -     数组 + 单链表(到达一定的数量是，转成红黑树)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-21-19-07-37-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-21-19-29-54-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-21-19-42-55-image.png)
> 
> hash表的好坏，根据‘散列均匀(<mark>node_num / arrary.length 的平均值</mark>  )’判断，所以hash函数要设计好

> 如图：
> 
>     hash 的结点node包含(.key,.hash,.value,.next);四个属性；
> 
> - 其中的hash：   hash = hash(key)      //有一个hash函数，负责将key转换成 ‘数组索引：hash值’
> 
> ---
> 
> - hash的特征，<mark>key无序</mark>，key唯一(也就是，多个相同的key，进行put的时候，数据是覆盖的，一个key只保存一个最后put的值)
>   
>   - 保证key唯一值，需要重写 equals ，还有 hashcode

###### hash表的 存储原理(图解)

![](/home/administrator/.config/marktext/images/2024-09-21-19-46-30-image.png)

> - 先 “ hashcode(key)  “ 获得 hash值，![](/home/administrator/.config/marktext/images/2024-09-21-21-17-42-image.png) 
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-21-21-19-08-image.png)
>   
>   因为hashcode的值不一样（Object的hashCode返回的是jvm内存地址），所有求得的索引也不一样，所有没有hash冲突，所equals判断结果使得执行插入，而不是覆盖
> 
> - hash值%arr.length 获得 <mark>数组索引</mark>，通过索引找数组下标，该数组下标为空NULL时，进行插入，不为空就比较(<mark>equals ： 比较 key的值)，通过equals比较的的返回值（true、flase）决定是覆盖还是尾部插入

###### 重写hashcode 和 equals ： 实现map-key的唯一性

- 需要重写 hashCode（<mark>Object.hash(key)</mark>）
  
  ![](/home/administrator/.config/marktext/images/2024-09-21-21-37-36-image.png)
  
  > 上述代码，是根据hash生成 hash值，可根据业务决定是要几个“条件”生成hash
  > 
  > 原理就是这个：![](/home/administrator/.config/marktext/images/2024-09-21-21-40-42-image.png)，**but**！！！不要自己写，使用提供的<mark>Objects.hash(  )</mark>生成的哈希散列更均匀

- 代码实现：
  
  User.java

```java
/**
 * @author : administrator
 * @created : 2024-09-21
**/
package com.test;
import java.util.*;
public class User{

    private String name;
    private int age;

    public void setName(String name){
        this.name = name;
    }
    public String getName(){
        return this.name;
    }
    public void setAge(int age){
        this.age = age;
    }
    public int getAge(){
        return this.age;
    }

    public User(){

    }

    public User(String name, int age){
        this.name = name;
        this.age = age;

    }
    //实现hash的key唯一的关键： 重写 hashCode()、equals( ) 
    @Override
    public boolean equals(Object o){
        if(this == o) return true;  //判断
        if(o == null || getClass() != o.getClass()) return false; //判断
        User user = (User) o;
        return (age == user.age) && Objects.equals(name,user.name); //判断    }

    @Override
    public int hashCode(){
        return Objects.hash(name,age);
    }

}
```

  HashMapTestUser.java

```java
/**
 * @author : administrator
 * @created : 2024-09-21
**/
package com.test;
import java.util.*;
import com.test.User; 
public class HashMapTestUser{

    public static void main (String[] args) {
        Map<User,Integer> userMap = new HashMap<>();
        userMap.put(new User("zdfdfdhangshan",100),100000);
        userMap.put(new User("zhangshan",100),100000);
        userMap.put(new User("zhangshan",100),100000);
        userMap.put(new User("wuzhongpwng",100),100000);
        userMap.put(new User("zhangshan",100),100000);
        userMap.put(new User("zhangshan",100),100000);
        userMap.put(new User("zhangshan",100),100000);
        userMap.put(new User("zhangshan",100),100000);

        Set<Map.Entry<User,Integer>> entrys = userMap.entrySet();
        for(var entry : entrys){
            System.out.println(entry.getKey().getName() +"<->"+ entry.getKey().getAge() + " : " + entry.getValue());
        }

    }
}
```

###### Map接口的常用方法

![](/home/administrator/.config/marktext/images/2024-09-21-14-57-03-image.png) 

> - remove()有返回值，可以选择接收：
>   
>   - V value = object.remove()
> 
> - Collection<V> valuse(); //只返回 key-value的value部分
>   
>   ```java
>     Collection<String> list = Students.values();
>     //Set<String> list = students.values();
>     //假设获取学生姓名；
>     Iterator<String> it = list.iterator();
>     while(it.hasNext(){
>        System.out.println(it.next());
>     }
>   ```
> 
> - Map的静态方法：生成键值对
> 
> ![](/home/administrator/.config/marktext/images/2024-09-21-15-07-40-image.png)
> 
> - keySet()
> 
> ```java
> Map<String,Integer> maps = new HashMap<>();
> maps.put("lishi",23);
> maps.put("wangwu",43);
> Object keys = maps.keySet();
> System.out.println(keys);
> //[lishi, wangwu] //获得键，存如set，so，key是唯一的
> //keySet() 获取到了key，那结合 get(),就可以获取到值
> for(var key : keys){
>     System.out.println(maps.get(key));
> }
> ```
> 
> - 补充：...
> - .entrySet(); //获取map对象
>   - 对象.entryKey()
>   - 对象.entryValue()

###### Map集合的遍历

```java
import java.util.*;
public class MapIterator{
    public static void main(String[] args){
        Map<String, Integer> map = new HashMap<>();
     c   map.put("a", 1);
        map.put("b", 2);
        map.put("c", 3);

// 使用 .entrySet() 可以获得整个map集合对象
        // // Using for-each loop
        // for (Map.Entry<String, Integer> entry : map.entrySet()) {
        //     System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
        // }

        // // Using Iterator
        // Iterator<Map.Entry<String, Integer>> iterator = map.entrySet().iterator();
        // while (iterator.hasNext()) {

        // }

        //Set<Sring> keys = map.keySet();
        // Iterator<String> it = keys.iterator();
        //...
        Iterator<String> it = map.keySet().iterator();
        while (it.hasNext()) {
            String key = it.next();
            System.out.println( key + " : " + map.get(key));
    }

        //使用for
        Set<String> keys = map.keySet();
        for(String key : keys){
            System.out.println( key + “ ： ” + map.get(key) );
        }  //for-each 是包装了Iterator的
}

}
```

###### <mark>更高效</mark>的遍历map集合的方式：

- 效率高是因为它是直接获取到对象，然后通过对象，使用getKey(),getvalue获得...

![](/home/administrator/.config/marktext/images/2024-09-21-16-02-22-image.png)

---

> 使用 Map的内部类(是接口，内部接口)：Entry<K,V>
> 
> - Map.Entry<String,Integer>  <--- 类型type
> 
> - @return : Set<Map.Entry<String,Integer>
> 
> ![](/home/administrator/.config/marktext/images/2024-09-21-15-55-26-image.png)

```java
//Set<>集合存放的是map的对象(单列)





import java.util.*;
public class MapIterator{
    public static void main(String[] args){
        Map<String, Integer> map = new HashMap<>();
        map.put("a", 1);
        map.put("b", 2);
        map.put("c", 3);

        Set<Map.Entry<String,Integer>> entrys = map.entrySet();
        //for-earch-loop                       //V key = map.keySet(); + map.get(key)
        for(Map.Entry<String,Integer> entry : entrys){
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        /*
        for(Map.Entry<String,Integer> entry ; map.entrySet() ){
            System.out.println(entry.getKey() + " : " + entry.getValue());

        }
        */


        // Iterator
        Iterator<Map.Entry<String,Integer>> iterator = entrys.iterator();
        while(iterator.hasNext()){
            Map.Entry<String,Integer> entry = iterator.next();
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
}

}
```

- Map key-value 可为 null，但是只能有一对，因为都一样的hashCode地址

#### HashMap

- hash表的改进（这个改进是相对于 java-8来的）

![](/home/administrator/.config/marktext/images/2024-09-22-14-35-05-image.png)

- hashMap的扩容： 2^n  ; 2,4,8,16,32,64,...
  
  - why? 
  
  - ![](/home/administrator/.config/marktext/images/2024-09-22-14-51-18-image.png)
  
  - so: 就是为了解决<mark>(减少)hash冲突的发生 - 散列分布均匀</mark>，和提高计算效率(保证是2的次幂才能使用位位运算)得到正确结果![](/home/administrator/.config/marktext/images/2024-09-22-14-55-53-image.png)
    
    对比Hashtable的解决方式：![](/home/administrator/.config/marktext/images/2024-09-22-21-38-15-image.png)

- 什么时候链转树，什么什么时候树转链

    ![](/home/administrator/.config/marktext/images/2024-09-22-14-39-36-image.png)

- hash表<mark>扩容</mark>：初始化 16的长度
  
    【】-[]->[]->[]->[]->null
    【】
    【】-[]->[]->
    【】
    {+}
    {+}

 对hashMap进行扩容是很耗成本的...(需要重新进行hash计算，重新将node链接...非常耗资源)：![](/home/administrator/.config/marktext/images/2024-09-22-18-52-52-image.png)       

所以，为了提高效率...

![](/home/administrator/.config/marktext/images/2024-09-22-18-53-31-image.png)

> 先进行预估（大概需要多少个），进行初始化个数
> 
> hashMap的扩容：使用到加载因子：<mark>0.75</mark>是测试得出的最优结果
> 
> (0.75 *  hashtable.length) 到达时，触发扩容

- 思考题：

![](/home/administrator/.config/marktext/images/2024-09-22-19-05-42-image.png)

> - 离 15 最近的2次幂是  16 = 2^4；
> 
> - 16 * 0.75 = 12(真实容量：因为到了12就进行扩容操作了) 

##### LinkedHashMap

> hashMap +  doubleLinkedList
> 
> - LinkedHashMap的结构：
> 
> ![](/home/administrator/.config/marktext/images/2024-09-22-20-04-58-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-22-20-04-26-image.png)
> 
> 就是靠 befor 和 after 实现的 可顺序
> 
> - LinkedHashMap 能有序的插入元素进入hash表：
> 
> ```java
> /**
>  * @author : administrator
>  * @created : 2024-09-22
> **/
> import java.util.*;
> 
> public class ExerciseLinkedHashMap{
> 
>     public static void main (String[] args) {
>        Map<String,String> mapName = new LinkedHashMap<>();
>         mapName.put("one","java");
>         mapName.put("two","python");
>         mapName.put("three","c++");
>         mapName.put("four","c");
>         mapName.put("five","node.js");
>         mapName.put("6","66");
> 
>         Set<Map.Entry<String,String>> entrys = mapName.entrySet();
>         Iterator<Map.Entry<String,String>> it = entrys.iterator();
>         while(it.hasNext()){
>             Map.Entry<String,String> entry = it.next();
>             System.out.println(entry.getKey() + " : " + entry.getValue());
>         }
> 
>     }
> }
> ```
> 
> 输出结果：
> 
> ```shell
> one : java
> two : python
> three : c++
> four : c
> five : node.js
> 6 : 66
> # 能做到，谁先put，谁就先在前
> ```

#### Hashtable:

> 和hashMap是一样的，但是 Hashtable是线程安全（有锁，需要等待）的，效率要差
> 
> ![](/home/administrator/.config/marktext/images/2024-09-22-21-40-56-image.png)

- Hashtable的put方法：（Hashtable的value不能为空）

![](/home/administrator/.config/marktext/images/2024-09-22-21-39-10-image.png)

##### Hashtable有自己的迭代器，（当然他也能用 Iterator<>)

<u>如何使用？因为特有嘛，所以要使用Hashtable类本身</u>

![](/home/administrator/.config/marktext/images/2024-09-22-21-48-44-image.png)

使用Map接口并没有Hashtable的独有迭代器

![](/home/administrator/.config/marktext/images/2024-09-22-21-49-19-image.png)

###### .Keys()；获取所有key的迭代器： Enumeration<T> 类型接收

![](/home/administrator/.config/marktext/images/2024-09-22-21-51-32-image.png)

![](/home/administrator/.config/marktext/images/2024-09-22-21-54-16-image.png)

> 和Iterator还是很像的...
> 
> - Enumeration<T> ： .key()方法的返回值类型

###### .elements();获取map的所有value的迭代器

![](/home/administrator/.config/marktext/images/2024-09-22-21-56-27-image.png)

![](/home/administrator/.config/marktext/images/2024-09-22-21-56-55-image.png)

##### Properties（继承的Hashtable）

> key 和 value 都是 String类型    ： 无范型，不支持范型，只有String    
> 
> - 主要用于 IO流
> 
> - 使用：
>   
>   1.setProperty() ； 底层就是 put();方法
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-22-22-10-58-image.png)
>   
>   2. getProperty();
>   
>   ![](/home/administrator/.config/marktext/images/2024-09-22-22-14-09-image.png)
> 
> - properties  使用的不是Key-value;   而是 name-value
> 
> ![](/home/administrator/.config/marktext/images/2024-09-22-22-16-21-image.png)
> 
> > 同专属迭代器：使用propertyNames()
> > 
> > - 结合 hasMoreElements()  + nextElement() 一起使用
> > 
> > ![](/home/administrator/.config/marktext/images/2024-09-22-22-17-34-image.png)
> > 
> > ![](/home/administrator/.config/marktext/images/2024-09-22-22-18-06-image.png)获取全部~~key~~ name 的迭代器
> > 
> > - 要想获取全部value的方式：
> > 
> > ![](/home/administrator/.config/marktext/images/2024-09-22-22-20-27-image.png)

- 小结：

![](/home/administrator/.config/marktext/images/2024-09-22-22-21-45-image.png)

#### TreeMap

- 树的概念：

![](/home/administrator/.config/marktext/images/2024-09-22-22-38-44-image.png)

![](/home/administrator/.config/marktext/images/2024-09-22-22-54-20-image.png)

> TrreMap就是基于红黑二叉树实现的：
> 
> - 自平衡：满足给定的约束（******）
> 
> ![](/home/administrator/.config/marktext/images/2024-09-22-22-56-00-image.png)

```java
/**
 * @author : administrator
 * @created : 2024-09-23
**/
import java.util.*;
public class TreeMapDemo{
    public static void main(String[] args){

        TreeMap<String,Integer> treeMap = new TreeMap<>();
        treeMap.put("zhanshna",34);
        treeMap.put("wangwu",23);
        treeMap.put("lishi",33);
        treeMap.put("wuzhonpeng",18);
        treeMap.put("benzhu",26);
        System.out.println("------ iterator ---------");
        Set<Map.Entry<String,Integer>> entrys = treeMap.entrySet();
        Iterator<Map.Entry<String,Integer>> it = entrys.iterator();
        while(it.hasNext()){
            Map.Entry<String,Integer> entry = it.next();
            System.out.println(entry.getKey() + " : " + entry.getValue());

        }
        System.out.println("----for:----------------");


        for(Map.Entry<String,Integer>  entry : entrys){
            System.out.println(entry.getKey() +  " ::::: " + entry.getValue());
        }

    }

}
```

> ❯ java TreeMapDemo
> ------
> 
> benzhu : 26
> lishi : 33
> wangwu : 23
> wuzhonpeng : 18
> zhanshna : 34
> ----for:----------------
> benzhu ::::: 26
> lishi ::::: 33
> wangwu ::::: 23
> wuzhonpeng ::::: 18
> zhanshna ::::: 34
> 
> -------------------------------- 结论： treeMap会排序输出（String 按 ascall码顺序，Integer等整形按 数字大小排序 asc）

###### TreeMap<mark>自定义类</mark>型（非String，Integer...），如何也能进行 排序？？

> 1.使用 compable<T> 实现:会破坏程序结构：适用于固定不变的比较，比如数字间比较
> 
> ![](/home/administrator/.config/marktext/images/2024-09-23-23-19-25-image.png)
> 
> **comparaTo()**  // 重写
> 
> ![](/home/administrator/.config/marktext/images/2024-09-23-23-18-34-image.png)
> 
> ![](/home/administrator/.config/marktext/images/2024-09-23-23-22-26-image.png)
> 
> ----

> 2. 使用 Comparator    ： 不会破坏 程序结构（不用在自定义类实现接口  comparable<T>）；<mark>更灵活</mark>
> - 使用 comparator 比较器实现，需要写一个类，实现comprator<T> : 满足ocp原则:重写 **compare()**方法
> 
> ![](/home/administrator/.config/marktext/images/2024-09-24-14-04-15-image.png)     
> 
> ... 和 Array - List 实现排序.sort() 是一样的处理方式    
> 
> - 如何使用呢？
> 
> ![](/home/administrator/.config/marktext/images/2024-09-24-14-12-11-image.png)
> 
> 传入一个比较器(手动编写：实现comparator<T>接口的 compara()方法的类...
> 
> ![](/home/administrator/.config/marktext/images/2024-09-24-14-11-51-image.png)
> 
> 使用的语法：![](/home/administrator/.config/marktext/images/2024-09-24-14-13-41-image.png)
> 
> - 小结： equals + hashCode ，实现 key唯一；comparator() and comparable()实现key-value  有排序

---

###### TreeMap<T> map = new TreeMap<>(paraments); 源码解析：

![](/home/administrator/.config/marktext/images/2024-09-24-14-19-09-image.png)

当然没有传入构造器参数是走的 comparable分支...(也就是：内部会找到自定义的实现comparable接口的comparaTo()方法...)

- 当然，可以使用 匿名方式实现：

```java
  Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name);
            }
        });
```

- 小结： 
  
  - [x] TreeMap : key 不能为null
  
  - [ ] Hashtable ： key 和 value 不能为null
  
  - [ ] Properties : key value 都不能为空...
  
  - [ ] Collection 的 subClass 的key-value 都可为空
  
  - [ ] ...
  
  ![](/home/administrator/.config/marktext/images/2024-09-24-15-13-41-image.png)
  
  > LinkedHashSet ,HashSet 可为null

#### Set （Map的延伸）

![](/home/administrator/.config/marktext/images/2024-09-24-15-15-31-image.png)![](/home/administrator/.config/marktext/images/2024-09-24-15-16-08-image.png)

##### HashSet

... 也和HashMap一样，要重写 equals 和 hashCode...才能实现key唯一

##### LinkedHashSet

...和 LinkedHashMap 一样， 内部数据结构就决定了是有序存入到结构内的

##### TreeSet

... 和 TreeMap也一样：

![](/home/administrator/.config/marktext/images/2024-09-24-15-27-11-image.png)

###### HashSet面试题

![](/home/administrator/.config/marktext/images/2024-09-24-18-04-41-image.png)

分析内部原理：(记住：存在哈希表内的，都是需要 hash值找索引的：<mark>hash值的取的</mark>(参数1 + 参数2 + 参数3 ...)。。。

![](/home/administrator/.config/marktext/images/2024-09-24-18-05-25-image.png)

## Collections-工具类

![](/home/administrator/.config/marktext/images/2024-09-24-18-45-00-image.png)

- 用于操作Collection实现的实现类...List...ArrayList... Set...

![](/home/administrator/.config/marktext/images/2024-09-24-18-21-02-image.png)

###### .Sotr()： 专门给List集合使用的...

![](/home/administrator/.config/marktext/images/2024-09-24-18-26-03-image.png)

- 如果想让自定义的类也能使用Collections.sort(),需要将自定义类实现一下comparable<T>接口,,,

![](/home/administrator/.config/marktext/images/2024-09-24-18-29-04-image.png)

- 也可以使用 comparator<> 比较器实现...  发现没有，和List的.sort()方法想对自定义类排序是一样的操作，1.要么 实现 comparable<ClassName>,2.要么实现 comparator<ClassName> 比较器：   （传入两个参数）

![](/home/administrator/.config/marktext/images/2024-09-24-18-33-31-image.png)

###### .shuffle()  洗牌：打乱

![](/home/administrator/.config/marktext/images/2024-09-24-18-35-29-image.png)

输出：    

![](/home/administrator/.config/marktext/images/2024-09-24-18-39-09-image.png)



###### Collections.其他...

![](/home/administrator/.config/marktext/images/2024-09-24-18-40-57-image.png)

> .reverse(); 会将原list集合进行修改...
> 
> ![](/home/administrator/.config/marktext/images/2024-09-24-18-43-12-image.png)



![](/home/administrator/.config/marktext/images/2024-09-24-18-47-01-image.png)

- 关于不可变集合，是会对原集合 有“副作用的” 需置null

![](/home/administrator/.config/marktext/images/2024-09-24-18-49-23-image.png)

...
