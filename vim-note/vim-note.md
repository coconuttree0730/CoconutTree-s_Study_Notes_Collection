# VIM

### vim的显示：

![](/home/administrator/.config/marktext/images/2024-09-13-15-26-17-image.png)

> tab > window == buffer
> 
> //一个屏幕上可以有多个tab，每个tab内可window出多个窗口，每个tab下可以显示buffer文件...

#### buffer （***）  ：

![](/home/administrator/.config/marktext/images/2024-09-13-15-30-54-image.png)

> : ls 
> 
> : reg
> 
> :b
> 
> :e 

##### [buffer]的切换：

![](/home/administrator/.config/marktext/images/2024-09-13-15-35-42-image.png)

#### window

> :vs
> 
> :sp
> 
> :vs <filename>   
> 
> （<ctrl>  w ）+  跳窗(hjkl)   :  实现多个窗口间跳转

#### tab

- tabnew <filename>   : 创建一个新的标签页

- 

![](/home/administrator/.config/marktext/images/2024-09-13-15-44-06-image.png)

![](/home/administrator/.config/marktext/images/2024-09-13-15-44-51-image.png)

> gt : tab间切换

## Insert

- i/I :前插入

- a/A:后插入

- c ： 修改

- x ： 删除(delete and insert) 一个char

- d    删除
  
  - dd : 删除一行
  
  - d<位置> ： d3l   \ d0  \  d$  \ ...

- r ：修改一个

- R：修改多个(连续)

- s: 删除一个char并插入编辑

- S：删除一行并插入编辑

- u : undo,撤销

- <ctrl> + r  : 回退

- 命令模式下；：e!  --> 不保存编辑，回退到打开文件时的样子

---

 [在insert-model内]：

- <CTRL> + h  :  删除一个char

- <CTRL> + w ：删除一个单词

- <CTRL> + u  ：删除一行

[shell-command]:

补充：

- <CTRL> + K  : 删除光标后面的字符

- <CTRL> + e :  移动到行后

- <CTRL> + a :  移动到行前      

---

- g i :  normal 时, <mark>快速跳转到最近编辑过的行</mark>，然后进行insert...      

###### [复制and粘贴]：

![](/home/administrator/.config/marktext/images/2024-09-15-15-27-24-image.png)

> 这是关于 python代码缩进对齐的解决方案...

---

## normal

##### [移动]：

![](/home/administrator/.config/marktext/images/2024-09-13-14-22-19-image.png)

##### [行间搜索移动]

![](/home/administrator/.config/marktext/images/2024-09-13-14-26-37-image.png)

- t{char} ：char前

- f{char} ：char上

##### [水平移动]

![](/home/administrator/.config/marktext/images/2024-09-13-14-30-58-image.png)

- '^'  ==> '<mark>0w'</mark>

##### [垂直移动]

![](/home/administrator/.config/marktext/images/2024-09-13-14-34-21-image.png)

vim内部：

- ‘  :help {}  ‘

- ' :help () '

##### [页面移动]

![](/home/administrator/.config/marktext/images/2024-09-13-14-38-58-image.png)

##### [快速删除&修改]

- C : 删除整行并插入

- S : ... 

- ciw : 只删除单词

- caw ：单词后的空格也删除

- dt{char} : 删除光标当前位置到(<mark>to</mark>) char 之间的字符

![](/home/administrator/.config/marktext/images/2024-09-13-14-59-22-image.png)

##### [查询]

![](/home/administrator/.config/marktext/images/2024-09-13-15-03-26-image.png)

> n/N  和  */#  都是基于先进行 / 或 ？ 【回车键】之后的...

##### 复制and粘贴

![](/home/administrator/.config/marktext/images/2024-09-15-15-22-53-image.png)  

## command

##### [搜索替换]（支持正则表达式）

- s 表示 ‘替换’ ==> sp

![](/home/administrator/.config/marktext/images/2024-09-13-15-09-33-image.png)

[flags]:

![](/home/administrator/.config/marktext/images/2024-09-13-15-17-46-image.png)

- 精确匹配某个单词：
  
  ```vim
  : %s/\< oldword \>/newword/[flags]   # 不留空格
  ```

## Visual

- ctrl + v  :矩形

- v ：单个

- shift + v ： 一行
  
  - 将选中的文本进行  大小写转换：<mark> shift + u</mark>

## 文本对象：Text-Objec

![](/home/administrator/.config/marktext/images/2024-09-15-15-11-58-image.png)

![](/home/administrator/.config/marktext/images/2024-09-15-15-18-02-image.png)

---

## vim寄存器

![](/home/administrator/.config/marktext/images/2024-09-15-15-30-54-image.png)

    

![](/home/administrator/.config/marktext/images/2024-09-15-15-35-00-image.png)

> - 不使用 <mark>“</mark>(register-name) 默认是 剪切到“<mark>无名寄存器</mark>”
> 
> 使用\\\\\\\\\\[Tips]：
> 
> - <mark>"a </mark> yy  : 复制一行到 a寄存器
> 
> - <mark>"b</mark> p : 将a寄存器的内容粘贴到光标位置

![](/home/administrator/.config/marktext/images/2024-09-15-15-38-32-image.png)

> ：%yank+     ：将当前文件选中的内容复制到系统剪贴板，使得除了vim之外的软件都可用该复制内容
> 
> \\\\\\\[Tips]:
> 
> - vim不一定支持 系统剪切板，使用 ` ：echo has('clipboard')`   返回结果是 `1` 表示支持...
>   
>   - ` 选中文本     `   `" + <vim-command>  `
>   
>   - 其他软件文本想复制到vim 也可这样： ` " + p  `

## 宏(marco)

- 是vim指令的集合

![](/home/administrator/.config/marktext/images/2024-09-15-15-50-27-image.png)

![](/home/administrator/.config/marktext/images/2024-09-15-15-51-30-image.png)

q {register-name} ----> command1 , command2, command3 ...   ---->  q

@ {register-name}

- 例子：给多行文本的头部和尾部添加 “”

![](/home/administrator/.config/marktext/images/2024-09-15-16-06-31-image.png)

1.<mark>qa</mark> ---> start

2.command..

3.<mark>q</mark> ---> finish 

--

4.shift + v  ; 

5.shift + g;  选中全部行

6.对全部行进行宏播放

命令行模式<mark>：normal @a</mark>

![](/home/administrator/.config/marktext/images/2024-09-15-16-09-11-image.png)

> :normal I"  --> 对选中行进行行首插入“
> 
> ...

## vim补全

![](/home/administrator/.config/marktext/images/2024-09-15-16-29-45-image.png)

> 补全代码需要 plug-in...

## vim配置

？vim可以配置什么

![](/home/administrator/.config/marktext/images/2024-09-15-19-13-23-image.png)

###### 有用配置：（在 /etc/vim/vimrc文件内编写）

- （1）键盘映射

![](/home/administrator/.config/marktext/images/2024-09-15-19-42-45-image.png)

> ctrl + (hjkl)：实现窗口切换
> 
> 使用：...

- （2）：文本格式化

![](/home/administrator/.config/marktext/images/2024-09-15-19-44-38-image.png)

> 命令模式：FormatJSON  可将.json文件格式化    

- （3）：设置leder键取代 <Esc>键

![](/home/administrator/.config/marktext/images/2024-09-15-20-08-12-image.png)

> jj : 从insert --> normal ...
> 
> ,w : 保存文本

- (4): 在没有权限的使用强制写入

![](/home/administrator/.config/marktext/images/2024-09-15-20-33-27-image.png)

### 映射

#### 基本模式映射

![](/home/administrator/.config/marktext/images/2024-09-15-20-13-36-image.png)

#### 其他映射

> 递归映射就不列举了...

![](/home/administrator/.config/marktext/images/2024-09-15-20-25-49-image.png)

 

### 安装插件

    
