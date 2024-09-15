# 异常处理

> 所有的异常类都是继承 Throwable (接口)
> 
>                               Throwable
> 
>                             /                    \
> 
>                  Exception                  Error
> 
>                   /               \                        \
> 
>  checkExcept...   RuntimeExcept...    (...):NullPointerError、ArithmeticError、 ...
> 
>           /|\                                  \              /
> 
>   IOE... ...                      uncheckException

## 异常产生/抛出(throws/throw)

- 自<mark>定义</mark>异常类(Class)：
  
  - extends :  Exception  //自定义的异常类需要使用(继承) Exception接口...
  
  - 生成构造方法(<mark>要有这两个</mark>)：1.有参数，2.无参数：

- <mark>使用</mark>：
  
  - **<mark>throw</mark>** new construction("Error Messages...");//这<u>调用的是有参数的</u>
  - **<mark>throws </mark>**用于将异常抛给调用者，由调用者进行 try-catch处理

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



- 新用法：
  
  ```java
  try(//申请资源自动回收处){ 
  
  }catch(XXException e){
  
  }//无需使用finally{}
  ```
