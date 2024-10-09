## Lambda表达式 ： OOF -> 面向函数编程

> Lambda表达式 只能 是 接口

?疑问？ 编译器怎么知道，我使用了lambda表达式，对应的是调用了哪个方法？

答：有控制只能使用某种类型； 某类型内有多个方法，那又是怎么知道调用的就是这个？： 使用了<mark> @FuntionInterface </mark>标注(“这个方法能用lambda表达式”)    

- 因为FuntionInterface 注解 规定接口内部只能有一个 可使用lambda的方法，确定了它的<mark>唯一性</mark>

如：    

![](/home/administrator/.config/marktext/images/2024-10-09-22-42-35-image.png)
