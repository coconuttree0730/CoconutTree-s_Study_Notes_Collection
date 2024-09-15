## Maven

### 依赖管理

- 引入第三方库(.jar)包时；（包通常都是一个依赖另一堆...）
  
  ![](/home/administrator/.config/marktext/images/2024-09-05-22-16-41-image.png)

- 通过配置pom.xml文件 ，进行自动包管理
  
  > Project Object model ...

## 项目构建

- 将多个java文件打包成jar包
  
  ![](/home/administrator/.config/marktext/images/2024-09-05-22-19-10-image.png)
  
  > mvn package   

---

### maven-能做什么？

        ![](/home/administrator/.config/marktext/images/2024-09-05-22-21-41-image.png)

---

### Pom.xml文件

> Project Object model ...  项目对象模型

#### 结构

。。。

- (maven 和 pom.xml )的关系 == ( make 和 makefile ) == (npm 和 package.json)

- jar包格式：（基本）
  
  ![](/home/administrator/.config/marktext/images/2024-09-06-14-53-15-image.png)
  
  > <groupId> : 包名：“com.wuzhonpeng.www”
  > 
  > <artifactID> : 项目名... 
  
  ##### 添加包（依赖），在pom.xml文件的<dependencies>内插入依赖<deoendency>
  
  （all）：
  
   ![](/home/administrator/.config/marktext/images/2024-09-06-14-53-53-image.png)
  
  - 指定依赖范围：
    
    ![](/home/administrator/.config/marktext/images/2024-09-06-19-41-06-image.png)
    
    compile \  test \runtime\ ...
    
    - 设置成 compile 可支持依赖传递
  
  > version：分为快照版and正式版；
  > 
  > 快照版   ：![](/home/administrator/.config/marktext/images/2024-09-06-14-56-30-image.png)
  > 
  > 正式版   ：![](/home/administrator/.config/marktext/images/2024-09-06-14-57-03-image.png)

#### 基本功能：

![](/home/administrator/.config/marktext/images/2024-09-05-22-27-56-image.png)

---

### Maven仓库

![](/home/administrator/.config/marktext/images/2024-09-05-22-30-38-image.png)

- 本地仓库
  
  路径：
  
  ![](/home/administrator/.config/marktext/images/2024-09-05-22-32-12-image.png)

- 服务器仓库（公司）

- 中央仓库（官方）

> 现在本地仓库找，本地没有时,在私服和官方库找,找到后存入本地库...

### 下载maven

1. 需要安装java 1.7 以上版本

2. 进入maven官网：http://maven.apache.org/   的download...

3. 选择下载版本

4. 解压：tar -xvzf  XXX.tar.gz

5. 配置环境变量：
   
   ```shell
      export MAVEN_HOME=/xxx/xxx/xxx
      export PATH=${MAVEN_HOME}/bin:$PATH
   ```

6. source .zshrc 

7. 验证： mvn -v 

#### 创建maven项目

- 生成项目

```shell
mvn -B archetype:generate -DgroupId=<包名> -DartifactId=<项目名> -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
```

> 图形化也可创建（idea）:
> 
> ![](/home/administrator/.config/marktext/images/2024-09-06-13-31-04-image.png)
> 
> - 配置maven：
>   
>   - ![](/home/administrator/.config/marktext/images/2024-09-06-13-39-52-image.png)
>     
>     ![](/home/administrator/.config/marktext/images/2024-09-06-13-41-13-image.png)
>     
>     - 生命周期(图形化-双击)包含：
>       
>       - <mark>clean</mark> ：清除class文件和jar包
>       
>       - <mark>site </mark>： 生成项目文档（静态网站：index.html）
>       
>       - <mark>default(</mark>
>         
>         - **validate**:验证pom.xml文件中的配置项正确性（有问题就报错）
>         
>         - **compile**：java源代码编译成class文件（target目录内）
>         
>         - **test**：单元测试（执行test目录中的测试用例）
>         
>         - **package**：打包(会自动先执行：mvn compile, 然后执行了 mvn test, 最后才自动打包成jar包)
>         
>         - **verify**： 检查打包生成的jar包是否正确,是否符合标准（不常用）
>         
>         - **install**：将jar安装到本地仓库
>         
>         - **deploy** : 将jar包上传到远程仓库（私服务仓库）<mark>)</mark> 。
>     
>     - 命令可以组合使用，如 mvn clean compile :现清除原本的class文件和jar包，然后重新编译 (CI/CD自动化脚本常用方式 )   

- 编项项目

```shell
mvn compile
```

> 编译过程可能会出现失败，重新配置一下pom.xml文件：(java版本和maven匹配)
> 
> <properties>
>      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
>      <maven.compiler.source>21</maven.compiler.source>
>      <maven.compiler.target>21</maven.compiler.target>
> </properties>

- 生成jar包

```shell
mvn package
```

- 运行项目

```shell
java -cp 'jar包位置' java项目完全类名（包名+类名）
```

---

#### 第三方jar包资源获取网站：

网址： https://mvnrepository.com/  

![](/home/administrator/.config/marktext/images/2024-09-06-15-01-58-image.png)

### 父子工程

> 公共资源管理

1. 创建父工程![](/home/administrator/.config/marktext/images/2024-09-06-20-20-58-image.png)

2. 打开它的pom.xml文件
   
   ![](/home/administrator/.config/marktext/images/2024-09-06-20-22-50-image.png)

3. 修改<packageing>内的值
   
   ![](/home/administrator/.config/marktext/images/2024-09-06-20-23-17-image.png)
   
   - pom:父工程，用于管理其他子工程
   
   - jar：java项目包
   
   - war：web项目包

4. 创建子工程（在父工程的项目内）
   
   ![](/home/administrator/.config/marktext/images/2024-09-06-20-27-23-image.png)

5. 子工程的pom.xml:
   
   ![](/home/administrator/.config/marktext/images/2024-09-06-20-28-33-image.png)
   
   子工程没有<groupId>，他继承<Parent>的<groupId>
   
   - 可在父工程创建多个子工程：
     
     当创建了多个子工程之后，父的pom.xml:
     
     ![](/home/administrator/.config/marktext/images/2024-09-06-20-32-01-image.png)

6. 完成
- 通过操作父工程（mvn clean；mvn package; mvn compile; ...）可将它的所有子项目一起执行；

- 依赖继承： 修改父工程的pom.xml（添加依赖项），会继承到子（子工程也会有该依赖，子工程的pom.xml无需在添加依赖配置）

---

###### 补充：

- <properties> 的使用：
  
  定义： `<xxxxx>111111</xxxxx>`
  
  使用： `<version>${xxxxxx}</version>`

![](/home/administrator/.config/marktext/images/2024-09-06-20-48-30-image.png)

------

## 私服仓库：

> 相当于 官方仓库的代理

![](/home/administrator/.config/marktext/images/2024-09-06-20-53-04-image.png)

优点2：私服仓库可存非开源jar包...

![](/home/administrator/.config/marktext/images/2024-09-06-20-54-13-image.png)

### 私服仓库的种类

![](/home/administrator/.config/marktext/images/2024-09-06-20-55-01-image.png)

。。。
