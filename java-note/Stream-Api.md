![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=YuXi1&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# 主要内容

1. Stream API的概述

2. 创建Stream的方式

3. Stream的中间操作

4. Stream的终止操作
   
   # 学习目标
   
   | **知识点**       | **要求** |
   | ------------- | ------ |
   | Stream API的概述 | 理解     |
   | 创建Stream的方式   | 掌握     |
   | Stream的中间操作   | 掌握     |
   | Stream的终止操作   | 掌握     |

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=Ce96G&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# Stream API的概述

## 什么是StreamAPI呢？

从JDK1.8开始，Java语言引入了一个全新的流式Stream API，StreamAPI把真正的函数式编程风格运用到Java语言中，使用StreamAPI可以帮我们更方便地操作集合，允许开发人员在不改变原始数据源的情况下对集合进行操作，这使得代码更加简洁、易读和可维护。
使用Stream API对集合数据进行操作，就类似于使用SQL执行的数据库查询，也可以使用Stream API来并行执行的操作。简而言之，Stream API提供了一种高效且易于使用的处理数据的方式。

## Stream和Collection的区别

Collection：是静态的内存数据结构，强调的是数据。
Stream API：是跟集合相关的计算操作，强调的是计算。
总结：Collection面向的是内存，存储在内存中；StreamAPI面向的是CPU，通过CPU来计算。

## Stream API的操作步骤

1. 第一步：创建Stream
   
   1. 通过数据源（如：集合、数组等）来获取一个Stream对象 。

2. 第二步：中间操作
   
   1. 对数据源的数据进行处理，该操作会返回一个Stream对象，因此可以进行链式操作。

3. 第三步：终止操作
   
   1. 执行终止操作时，则才会真正执行中间操作，并且并返回一个计算完毕后的结果。
      
      ## Stream API的重要特点

4. Stream自己不会存储元素，只能对元素进行计算。

5. Stream不会改变数据对象，反而可能会返回一个持有结果的新Stream。

6. Stream上的操作属于延迟执行，只有等到用户真正需要结果的时候才会执行。

7. Stream一旦执行了终止操作，则就不能再调用其它中间操作或终止操作了。

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=PEkV3&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# 创建 Stream的方式

## 通过Collection接口提供的方法

通过Collection接口提供的stream()方法来创建Stream流。

```java
List<String> list = Arrays.asList("aa", "bb", "cc");
Stream<String> stream = list.stream();
```

## 通过Arrays类提供的方法

通过Arrays类提供的stream()静态方法来创建Stream流。

```java
String[] arr1 = {"aa", "bb", "cc"};
Stream<String> stream = Arrays.stream(arr1);

int[] arr2 = {11, 22, 33, 44};
IntStream intStream = Arrays.stream(arr2);

long[] arr3 = {11, 22, 33, 44};
LongStream longStream = Arrays.stream(arr3);

double[] arr4 = {1.0, 2.0, 3.0};
DoubleStream doubleStream = Arrays.stream(arr4);
```

注意：Stream、IntStream、LongStream和DoubleStream都继承于BaseStream接口。

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=rrTiE&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 使用Stream接口提供的方法

通过Stream接口提供的of(T... values)静态方法来创建Stream流。

```java
Stream<String> stringStream = Stream.of("aa", "bb", "cc");
Stream<Integer> integerStream = Stream.of(11, 22, 33, 44);
```

## 顺序流和并行流的理解

在前面获得Stream对象的方式，我们都称之为“顺序流”，顺序流对Stream元素的处理是单线程的，即一个一个元素进行处理，处理数据的效率较低。
如果Stream流中的数据处理没有顺序要求，并且还希望可以并行处理Stream的元素，那么就可以使用“并行流”来实现，从而提高处理数据的效率。
一个普通Stream转换为可以并行处理的Stream非常简单，只需要用调用Stream提供的parallel()方法进行转换即可，这样就可以并行的处理Stream的元素。那么，我们不需要编写任何多线程代码就可以享受到并行处理带来的执行效率的提升。
【示例】把顺序流转化为并行流

```java
// 创建一个“顺序流”Stream对象
Stream<String> stream = Stream.of("aa", "bb", "cc");
// 验证：stream是否为并行流
System.out.println(stream.isParallel());         // 输出：false
// 将Stream对象转化为“并行流”
// 注意：parallel()方法返回的就是“方法的调用者对象”
Stream<String> parallelStream = stream.parallel();
System.out.println(stream == parallelStream);    // 输出：true
// 验证：stream是否为并行流
System.out.println(stream.isParallel());         // 输出：true
```

在Collection接口中，还专门提供了一个parallelStream()方法，用于获得一个并行流。
【示例】使用parallelStream()方法获得一个并行流

```java
List<String> list = Arrays.asList("aa", "bb", "cc");
// 创建一个“并行流”Stream对象
Stream<String> stream = list.parallelStream();
// 验证：stream是否为并行流
System.out.println(stream.isParallel()); // 输出：true
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=huh0z&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# Stream API的中间操作

中间操作属于惰式执行，直到执行终止操作才会真正的进行数据的计算，此处调用中间操作只会返回一个标记了该操作的新Stream对象，因此可以进行链式操作。
在后续的操作中，我们调用StudentData类的getStudentList()静态方法，则就能获得一个存储Student对象的List集合，其代码实现如下：

```java
public class Student {
    private String name;
    private int age;
    private String sex;
    private String city;

    public Student() {}
    public Student(String name, int age, String sex, String city) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.city = city;
    }
    /*setter和getter方法省略*/
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                ", city='" + city + '\'' +
                '}';
    }
}
public class StudentData {
    /**
     * 获得一个存储Student对象的List集合
     */
    public static List<Student> getStudentList() {
        ArrayList<Student> list = new ArrayList<>();
        list.add(new Student("张三", 21, "男", "武汉"));
        list.add(new Student("李四", 18, "女", "重庆"));
        list.add(new Student("王五", 25, "女", "成都"));
        list.add(new Student("赵六", 22, "男", "武汉"));
        list.add(new Student("王麻子", 16, "女", "成都"));
        return list;
    }
}
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=dHsjx&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 筛选（filter）

筛选（filter），按照一定的规则校验流中的元素，将符合条件的元素提取到新的流中的操作。该操作使用了Stream接口提供的“Stream<T> filter(Predicate<? super T> predicate);”方法来实现。
【示例】使用筛选的案例

```java
// 需求：筛选出年龄大于20的学生对象
Stream<Student> stream1 = StudentData.getStudentList().stream();
stream1.filter(stu -> stu.getAge() > 20).forEach(System.out :: println);
// 需求：筛选出字符串长度大于3的元素
Stream<String> stream2 = Stream.of("hello", "too", "like", "ande");
stream2.filter(str -> str.length() > 3).forEach(System.out :: println);
```

## 映射（map）

映射（map），将一个流的元素按照一定的映射规则映射到另一个流中。该操作使用了Stream接口提供的“<R> Stream<R> map(Function<? super T, ? extends R> mapper);”方法来实现。
【示例】使用映射的案例

```java
// 需求：把字符串中的字母全部转化为大写
Stream<String> stream1 = Stream.of("hello", "too", "like", "ande");
// stream1.map(str -> str.toUpperCase()).forEach(System.out :: println);
stream1.map(String :: toUpperCase).forEach(System.out :: println);

// 需求：获得集合中所有学生的名字
Stream<Student> stream2 = StudentData.getStudentList().stream();
// stream2.map(stu -> stu.getName()).forEach(System.out :: println);
stream2.map(Student :: getName).forEach(System.out :: println);

// 需求：获得集合中性别为男的学生名字
// 思路：先筛选，后映射
Stream<Student> stream3 = StudentData.getStudentList().stream();
stream3.filter(stu -> stu.getSex().equals("男")).map(Student :: getName).forEach(System.out :: println);
```

在Stream接口中，可以实现“将多个集合中的元素映射到同一个流中”，该操作使用了Stream接口提供的“<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);”方法来实现。
【示例】将多个集合中的元素映射到同一个流中

```java
// 需求：将两个集合中的元素映射到同一个流中
List<String> list1 = new ArrayList<>();
list1.add("aa");
list1.add("bb");
list1.add("cc");

List<String> list2 = new ArrayList<>();
list2.add("dd");
list2.add("ee");
list2.add("ff");

Stream<List<String>> stream = Stream.of(list1, list2);
stream.flatMap(List<String>::stream).forEach(System.out::println);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=nJWZ5&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 除重（distinct）

除重（distinct），也就是除去重复的元素，底层使用了hashCode()和equals(Object obj)方法来判断元素是否相等。该操作使用了Stream接口提供的“Stream<T> distinct();”方法来实现。
【示例】演示除重的操作

```java
// 需求：除去重复的元素
Stream.of(11, 22, 33, 44, 33).distinct().forEach(System.out :: println);

// 需求：除去重复的学生（除重后输出学生对象）
StudentData.getStudentList().stream().distinct().forEach(System.out :: println);

// 需求：除去年龄相同的学生（除重后输出学生年龄）
// 思路：先映射，后除重
StudentData.getStudentList().stream().map(Student :: getAge).distinct().forEach(System.out :: println);
```

## 排序（sorted）

排序（sorted），也就是对元素执行“升序”或“降序”的排列操作。在Stream接口中提供了“Stream<T> sorted();”方法，专门用于对元素执行“自然排序”，使用该方法则元素对应的类就必须实现Comparable接口。
【示例】使用自然排序的案例

```java
// 需求：对元素执行“升序”排序
Stream.of(4, 1, 3, 6, 2, 5).sorted().forEach(System.out :: println);

// 需求：按照学生的年龄执行“升序”排序（排序后输出学生对象）
StudentData.getStudentList().stream().sorted().forEach(System.out :: println);

// 需求：按照学生的年龄执行“升序”排序（排序后输出学生年龄）
StudentData.getStudentList().stream().map(Student :: getAge).sorted().forEach(System.out :: println);
```

在Stream接口中还提供了“Stream<T> sorted(Comparator<? super T> comparator);”方法，专门用于对元素执行“指定排序”，这样就能对某一个类设置多种排序规则。
【示例】使用指定排序的案例

```java
// 需求：对元素执行“升序”排序
Stream.of(4, 1, 3, 6, 2, 5).sorted(Integer :: compare).forEach(System.out :: println);

// 需求：按照学生的年龄执行“降序”排序（排序后输出学生对象）
StudentData.getStudentList().stream().sorted((stu1, stu2) -> stu2.getAge() - stu1.getAge()).forEach(System.out :: println);

// 需求：按照学生的年龄执行“升序”排序（排序后输出学生年龄）
StudentData.getStudentList().stream().map(Student :: getAge).sorted(Integer :: compare).forEach(System.out :: println);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=cKtTs&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 合并（concat）

合并（concat），也就是将两个Stream合并为一个Stream，此处使用Stream接口提供的“public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)”静态方法来实现。
【示例】将两个Stream合并为一个Stream。

```java
Stream<String> stream1 = Stream.of("aa", "bb", "cc");
Stream<String> stream2 = Stream.of("11", "22", "33");
Stream.concat(stream1, stream2).forEach(System.out :: println);
```

## 截断和跳过

跳过（skip），指的就是跳过n个元素开始操作，此处使用Stream接口提供的“Stream<T> skip(long n);”方法来实现。
截断（limit），指的是截取n个元素的操作，此处使用Stream接口提供的“Stream<T> limit(long maxSize);”方法来实现。
【示例】从指定位置开始截取n个元素

```java
// 需求：从索引为2的位置开始截取3个元素
Stream.of(11, 22, 33, 44, 55, 66).skip(2).limit(3).forEach(System.out :: println);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=b9XL9&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

# Stream API的终止操作

触发终止操作时才会真正执行中间操作，终止操作执行完毕会返回计算的结果，并且终止操作执行完毕那么操作的Stream就失效，也就是不能再执行中间操作或终止操作啦。

## 遍历（forEach）

遍历（forEach），使用Stream接口提供的“void forEach(Consumer<? super T> action);”方法来遍历计算的结果。
【示例】遍历操作的案例

```java
List<Student> list = StudentData.getStudentList();
// 遍历所有的元素
list.stream().forEach(System.out :: println);
// 遍历学生年龄大于20的元素
list.stream().filter(stu -> stu.getAge() > 20).forEach(System.out :: println);
```

## 匹配（match）

匹配（match），就是判断Stream中是否存在某些元素，Stream接口提供的匹配方法如下：

1. boolean allMatch(Predicate<? super T> predicate);  检查是否匹配所有的元素
2. boolean anyMatch(Predicate<? super T> predicate);  检查是否至少匹配一个元素
3. boolean noneMatch(Predicate<? super T> predicate); 检查是否一个元素都不匹配
4. Optional<T> findFirst(); 获得第一个元素

注意：此处的Optional是一个值的容器，可以通过get()方法获得容器的值。
【示例】匹配操作的案例

```java
List<Student> list = StudentData.getStudentList();
// 需求：匹配学生名字是否都为“王五”
boolean all = list.stream().allMatch(stu -> stu.getName().equals("王五"));
System.out.println("检查是否匹配所有的元素：" + all);
// 需求：匹配学生名字是否至少有一个为“王五”
boolean any = list.stream().anyMatch(stu -> stu.getName().equals("王五"));
System.out.println("检查是否至少匹配一个元素：" + any);
// 需求：匹配学生名字中是否全部都没有“王五”
boolean none = list.stream().noneMatch(stu -> stu.getName().equals("王五"));
System.out.println("检查是否一个元素都不匹配：" + none);
// 需求：获得第一个学生
Student firstStu = list.stream().findFirst().get();
System.out.println(firstStu);
// 需求：获得第四个学生
// 思路：跳过前面3个学生，然后再获得第一个元素
Optional<Student> skipStu = list.stream().skip(3).findFirst();
System.out.println(skipStu);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=Gh3wO&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 归约（reduce）

归约（reduce），将所有元素按照指定的规则合并成一个结果。在Stream接口中，常用的归约方法如下：

1. Optional<T> reduce(BinaryOperator<T> accumulator);
2. T reduce(T identity, BinaryOperator<T> accumulator);

【示例】归约操作的案例

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
// 需求：获得集合中所有元素“相加”的结果
// Integer sum = list.stream().reduce((x, y) -> x + y).get();
Integer sum = list.stream().reduce(Integer :: sum).get();
System.out.println(sum);
// 需求：获得集合中所有元素“相乘”的结果
Integer result = list.stream().reduce((x, y) -> x * y).get();
System.out.println(result);
// 需求：获得最大长度的元素
String str = Stream.of("I", "love", "you", "too").reduce((str1, str2) -> str1.length() > str2.length() ? str1 : str2).get();
System.out.println(str);
// 需求：获得所有学生的总年龄
Integer sumAge = StudentData.getStudentList().stream().map(Student::getAge).reduce((age1, age2) -> age1 + age2).get();
System.out.println(sumAge);
// 需求：获得10和集合中所有元素“相加”的结果
Integer sum1 = list.stream().reduce(10, Integer :: sum);
System.out.println(sum1);
```

reduce操作可以实现从一组元素中生成一个值，而max()、min()、count()等方法都属于reduce操作，将它们单独设为方法只是因为常用，在Stream接口中这些方法如下：
long count(); 获得元素的个数
Optional<T> max(Comparator<? super T> comparator); 获得最大的元素
Optional<T> min(Comparator<? super T> comparator); 获得最小的元素

【示例】获得最大、最小和元素的个数

```java
List<Student> list = StudentData.getStudentList();
// 需求：获得元素的个数
long count = StudentData.getStudentList().stream().count();
System.out.println(count);

// 需求：获得年龄“最大”的学生
Student maxStu = list.stream().max((stu1, stu2) -> stu1.getAge() - stu2.getAge()).get();
System.out.println(maxStu);
// 需求：获得学生的“最大”年龄
Integer maxAge = list.stream().map(Student::getAge).max(Integer::compare).get();
System.out.println(maxAge);

// 需求：获得年龄“最小”的学生
Student minStu = list.stream().min((stu1, stu2) -> stu1.getAge() - stu2.getAge()).get();
System.out.println(minStu);
// 需求：获得学生的“最小”年龄
Integer minAge = list.stream().map(Student::getAge).min(Integer::compare).get();
System.out.println(minAge);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=ZYTlY&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 收集（collect）

收集（collect），可以说是内容最繁多、功能最丰富的部分了。从字面上去理解，就是把一个流收集起来，最终可以是收集成一个值也可以收集成一个新的集合。
调用Stream接口提供的“<R, A> R collect(Collector<? super T, A, R> collector);”方法来实现收集操作，并且参数中的Collector对象大都是直接通过Collectors工具类获得，实际上传入的Collector决定了collect()的行为。

### 归集（toList/toSet/toMap）

因为Stream流不存储数据，那么在Stream流中的数据完成处理后，如果需要把Stream流的数据存入到集合中，那么就需要使用归集的操作。在Collectors提供的toList、toSet和toMap比较常用，另外还有Collectors提供的toCollection等比较复杂一些的用法。
【示例】演示toList、toSet和toMap的实现

```java
List<String> stringList = Arrays.asList("I", "love", "you", "too");
// 需求：将Stream转化为List集合
List<String> list = stringList.stream().collect(Collectors.toList());
System.out.println(list);
// 需求：将Stream转化为Set集合
Set<String> set = stringList.stream().collect(Collectors.toSet());
System.out.println(set);
// 需求：将Stream转化为Map集合
// 明确：每个元素以“:”来分割，左边的为key，右边的为value
Stream<String> stream = Stream.of("张三:成都", "李四:武汉", "王五:重庆");
Map<String, String> map = stream.collect(Collectors.toMap(str -> str.substring(0, str.indexOf(":")), str -> str.substring(str.indexOf(":") + 1)));
map.forEach((k, v) -> System.out.println("key：" + k + "，value：" + v));
```

在以上的代码中，我们将Stream流中计算的数据转化为List和Set集合时，此时并没有明确存储数据对应集合的具体类型，想要明确存储数据对应集合的具体类型，则就需要使用toCollection来实现。
【示例】演示toCollection的实现

```java
List<String> list = Arrays.asList("I", "love", "you", "too");
// 需求：将Stream转化为ArrayList集合
ArrayList<String> arrayList = list.stream().collect(Collectors.toCollection(ArrayList::new));
System.out.println(arrayList);
// 需求：将Stream转化为LinkedList集合
LinkedList<String> linkedList = list.stream().collect(Collectors.toCollection(LinkedList::new));
System.out.println(linkedList);
// 需求：将Stream转化为HashSet集合
HashSet<String> hashSet = list.stream().collect(Collectors.toCollection(HashSet::new));
System.out.println(hashSet);
// 需求：将Stream转化为TreeSet集合
TreeSet<String> treeSet = list.stream().collect(Collectors.toCollection(TreeSet::new));
System.out.println(treeSet);
```

【示例】获得年龄大于20岁的女同学，然后返回按照年龄进行升序排序后的List集合

```java
List<Student> list = StudentData.getStudentList();
ArrayList<Student> arrayList =
 list.stream().filter(stu -> stu.getAge() > 18). // 过滤年龄小于等于18的学生
 filter(stu -> stu.getSex().equals("女")). // 过滤男性学生
 sorted(Comparator.comparing(Student::getAge)). // 按照年龄执行升序排序
 collect(Collectors.toCollection(ArrayList::new)); // 转化为ArrayList存储
arrayList.forEach(System.out :: println);
```

在归集的知识点中，我们实现了将Stream中计算的数据转化为集合或Map，那么能否将Stream中计算的数据转化为数组呢？答案是可以的，我们可以使用Stream提供的toArray静态方法来实现。
【示例】将Stream中计算的数据转化为数组

```java
List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
// 需求：将Stream转化为数组
Object[] array = list.stream().toArray();
System.out.println(Arrays.toString(array));
// 需求：将Stream转化为“指定类型”的数组
String[] stringArray = list.stream().toArray(String[]::new);
System.out.println(Arrays.toString(stringArray));
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=ViFxh&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 统计（counting/averaging）

Collectors提供了一系列用于数据统计的静态方法：

1. 计数：counting
2. 平均值：averagingInt、averagingLong、averagingDouble
3. 最值：maxBy、minBy
4. 求和：summingInt、summingLong、summingDouble
5. 统计以上所有：summarizingInt、summarizingLong、summarizingDouble

【示例】对学生的年龄进行统计

```java
List<Student> list = StudentData.getStudentList();
// 需求：获得元素的个数
Long count = list.stream().collect(Collectors.counting());
System.out.println(count);
// 需求：获得学生的平均年龄
Double averAge = list.stream().collect(Collectors.averagingDouble(Student::getAge));
System.out.println(averAge);
// 需求：获得最大年龄的学生
Student stu = list.stream().collect(Collectors.maxBy((stu1, stu2) -> stu1.getAge() - stu2.getAge())).get();
System.out.println(stu);
// 需求：获得所有学生年龄之和
Long sum = list.stream().collect(Collectors.summingLong(Student::getAge));
System.out.println(sum);
// 需求：获得年龄的所有的信息
IntSummaryStatistics collect = list.stream().collect(Collectors.summarizingInt(Student::getAge));
System.out.println(collect);
```

![](https://cdn.nlark.com/yuque/0/2023/jpeg/21376908/1692002570088-3338946f-42b3-4174-8910-7e749c31e950.jpeg#averageHue=%23f9f8f8&from=url&id=Mkx2s&originHeight=78&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

### 分组（groupingBy）

分组（groupingBy），将Stream按条件分为两个Map，比如按照学生年龄分为两个Map集合。
【示例】按照学生性别分为两个Map集合

```java
List<Student> list = StudentData.getStudentList();
// 需求：按照学生性别进行分组
Map<String, List<Student>> map = list.stream().collect(Collectors.groupingBy(Student::getSex));
map.forEach((k, v) -> System.out.println("key：" + k + "，value：" + v));
```

### 接合（joining）

接合（joining），把Stream计算的数据按照一定的规则进行拼接。
【示例】获得所有学生的名字拼接成一个字符

```java
List<Student> list = StudentData.getStudentList();
// 需求：将所有学生的姓名连接成一个字符串，每个名字之间以“,”连接
String allName = list.stream().map(Student::getName).collect(Collectors.joining(", "));
System.out.println(allName);
```
