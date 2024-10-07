# 异常处理

> ***当程序出现异常，<mark>会结束程序执行</mark>，并抛出异常信息...
> 
>    BUT:使用 try-catch处理之后，程序的后续代码还会继续执行...!!!

---

- 异常类关系：

> 所有的异常类都是继承 Throwable (接口)
> 
>                               **Throwable**
> 
>                             /                    \
> 
>                  Exception                  Error
> 
>                   /               \                        \
> 
>  <mark>checkExcept...</mark>   RuntimeExcept...    (...):NullPointer**Error**、ArithmeticError、 ...
> 
>           /|\                                  \              /
> 
>   IOE... ...                     <mark> uncheckException</mark>

---



## 异常产生/抛出(throws/throw)

###### 自定义异常类(Class)：

- extends :  Exception  //自定义的异常类需要使用(继承：<u>extends</u> ) **Exception** 接口...

- 生成构造方法(<mark>要有这两个</mark>)：
  
  1.有参数: MyException("message ...")
  
  2.无参数：MyException( );

- <mark>使用</mark>：
  
  - **<mark>throw</mark>** new construction("Error Messages...");//这<u>调用的是有参数的</u>
  - <mark>throws </mark>用于将异常<u>抛给调用者</u>，由调用者进行 try-catch处理

---

## 异常处理(try-catch-finally)

- 用法：
  
  > <mark>try</mark>{
  > 
  > }<mark>catch</mark>(Exception e){
  > 
  >     e.xxxxxx
  > 
  > }catch(Exception e){
  > 
  >     e.xxxxxxx
  > 
  >     System.err.println("error ...");
  > 
  > }<mark>finally</mark>{
  > 
  > //........
  > 
  > }
  
  ###### 补充：

- 新用法：try - with - resource
  
  ```java
  try(//申请资源自动回收处){ 
  
  }catch(XXException e){
  
  }//无需使用finally{}
  ```
