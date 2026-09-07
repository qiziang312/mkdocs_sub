
# HELLO！！！
# 欢迎来学Manven

Java语法
git命令
Maven   <——目前学到这了欧
Nexus 私服
spring
mySQL
redis
苍穹外卖
黑马点评

# 一、Maven下载

Maven下载网址

[Download Apache Maven – Maven](https://maven.apache.org/download.cgi#CurrentMaven)

下载最新版：
	[apache-maven-3.9.16-bin.tar.gz](https://dlcdn.apache.org/maven/maven-3/3.9.16/binaries/apache-maven-3.9.16-bin.tar.gz)

1.点击浏览器右上下载，打开文件，下载完成之后打开文件所在位置，7-zip解压获得apache-maven-3.9.16文件夹，在C盘中新建tools文件夹，并把apache-maven-3.9.16粘贴进来

2.打开系统环境变量，（需要以管理员身份运行）点击环境变量（N），看系统变量（S），点击新建

变量名：MAVEN_HOME
变量值：C:\tools\apache-maven-3.9.16
        ps：apache-maven-3.9.16的绝对路径

3.在系统变量找到Path并点击，点击，之后点新建
写C:\tools\apache-maven-3.9.16\bin

4.powershell运行
```powershell
mvn -v
```
成功后会显示
```powershell
PS C:\Users\z005banx> mvn -v
Apache Maven 3.9.16 (2bdd9fddda4b155ebf8000e807eb73fd829a51d5)
Maven home: C:\tools\apache-maven-3.9.16
Java version: 26.0.1, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-26.0.1
Default locale: zh_CN, platform encoding: UTF-8
OS name: "windows 11", version: "10.0", arch: "amd64", family: "windows"
```

# 二、下载IDEA

IDEA下载网址
[下载 IntelliJ IDEA](https://www.jetbrains.com.cn/idea/download/?section=windows)

# 三、配置MAVEN

## 1、打开文件

在idea里打开apache-maven-3.9.16并找到settings.xml
```
apache-maven-3.9.16  C:\tools\apache-maven-3.9.16
├── .idea
├── bin
├── boot
├── conf
│   ├── logging
│   ├── settings.xml    <----打开这个文件
│   └── toolchains.xml  
├── lib
├── LICENSE
├── NOTICE
└── README.txt
```

## 2、修改settings.xml

在55行左右空白部分粘贴如下代码:

```xml
<localRepository>C:\tools\maven_repository</localRepository>
```

## 3.配置mirror镜像仓库

147-173行，照下面配置
```xml
<mirrors>  
  <!-- 
   此处为一大堆注释
   此处为一大堆注释
   此处为一大堆注释 
   -->  
  <mirror>  
    <id>maven-default-http-blocker</id>  
    <mirrorOf>external:http:*</mirrorOf>  
    <name>Pseudo repository to mirror external repositories initially using HTTP.</name>  
    <url>http://0.0.0.0/</url>  
    <blocked>true</blocked>  
  </mirror>  
  <mirror>  
    <id>aliyunmaven</id>  
    <name>阿里云公共Maven仓库</name>  
    <url>https://maven.aliyun.com/repository/public</url>  
    <mirrorOf>*</mirrorOf>  
  </mirror>  
</mirrors>
```

## 4.配置profiles

196行左右找到profiles按以下文件配置
```xml
<profiles>
   <!-- 
   此处为一大堆注释
   此处为一大堆注释
   此处为一大堆注释 
   -->
   
   <!-- 
   此处为一大堆注释
   此处为一大堆注释
   此处为一大堆注释 
   -->
   <profile>  
    <id>jdk-1.8</id>  
    <activation>  
      <activeByDefault>true</activeByDefault>  
    </activation>  
    <properties>  
      <maven.compiler.source>1.8</maven.compiler.source>  
      <maven.compiler.target>1.8</maven.compiler.target>  
      <maven.compiler.release>8</maven.compiler.release>  
      <maven.compiler.encoding>UTF-8</maven.compiler.encoding>  
    </properties>  
  </profile>  
</profiles>
```

## 5.配置activeProfiles

278行左右，在</profiles>下和</settings>上面按如下代码配置
```xml    
<!-- 
此处为一堆注释
此处为一堆注释
-->  
<activeProfiles>  
  <activeProfile>jdk-1.8</activeProfile>  
</activeProfiles>  
```

# 四、终端创建项目教学(可不做)

打开或新建一个空文件并运行如下命令：

官方命令（CMD可运行）
```shell
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=false
```

1. **-DgroupId** 项目组织 / 包名，例：`com.my`，对应代码包路径
2. **-DartifactId** 项目名称，生成文件夹、最终 jar 包都以此命名
3. **-DarchetypeArtifactId** 选择项目模板：

- `maven-archetype-quickstart`：普通 Java 项目
- `maven-archetype-webapp`：Java Web 网页项目

4. **-DarchetypeVersion** 模板版本，可省略，自动拉最新
5. **-DinteractiveMode=false** 关闭交互式弹窗，一键静默生成项目

修改后
`com.demo`（包名）
`demo1/web1`（项目名）

***关键说明

- 报错原因：PowerShell 会自动拆分 `-Dxxx=xxx` 格式参数，导致 Maven 收不到完整指令，识别不出是新建项目命令，强制校验 pom.xml 报错
- `--%`：核心修复点，让 PowerShell 停止参数解析，原样把内容传给 Maven
- `-B`：非交互批量模式，是新版 Maven 替代 `-DinteractiveMode=false` 的标准写法
- `com.demo`（包名）
- `demo1/web1`（项目名）

CMD/Powershell通用
```shell
mvn --% archetype:generate -B -DgroupId=com.demo -DartifactId=demo1 -DarchetypeArtifactId=maven-archetype-quickstart
```
完成后会创建demo1文件夹

# 五、在IDEA中使用Maven

1.创建工程
IDEA中依次点击   文件   新建  项目 生成器 Maven Archetype

名称                    demo
位置                   ~\Desktop\deskproject\Maven
Archetype          org.apache.maven.archetypes:maven-archetype-quickstart
高级设置
	组id                 com.study.maen
	工件                 demo

其他默认，点击创建


2.
IDEA中依次点击  文件 设置    构建、执行、部署   构建工具   Maven 

Manven 主路径（H）： 点击    ...    选择C:\tools\apache-maven-3.9.16文件夹  成功后显示（版本：3.9.16

用户设置文件（S）：      点击重写选择C:\tools\apache-maven-3.9.16\conf\settings.xml

其他默认点击确定


# 六、Maven的生存周期和插件（讲解）

在IDEA里打开demo（刚刚创建的项目）点击右侧的  m  图标，并打开demo
会看到：
	声命周期
	插件
	依赖项
	仓库

### 1.生命周期


***！！！！！！！！！这是Maven的核心功能！！！！！！！！！！***


点击任意阶段，**前面所有阶段会自动顺序执行** 例：点`package` → 自动执行 `validate→compile→test→package`

`validate → compile → test → package → verify → install → deploy`

1. clean：单独生命周期，清空`target`编译文件夹
2. validate：校验项目配置
3. compile：编译 Java 源码
4. test：执行单元测试
5. package：打包为 jar 包
6. verify：校验打包完整性
7. install：安装到本地仓库
8. deploy：推送至远程私服
9. site：生成项目文档站点（极少使用）

### 2. 插件 

生命周期只是**执行流程框架，本身无功能**，真正干活的是插件：
**点击生命周期------>插件------->完成操作**

### 3.依赖项 & 仓库

1. **依赖项**：当前项目引入的第三方包（图中 JUnit 测试依赖），在`pom.xml`里配置
2. **仓库**

- local：本地仓库路径 `C:\tools\maven_repository`
- central：Maven 官方中央远程仓库，用来下载插件和依赖 jar 包

# 七、Manven坐标（讲解）

查看项目demo下的pom.xml文件，
我们每需要一个想让Maven下载的jar包，就在这里加一个坐标
可以看到：

```xml
<!-- 每一个第三方jar都靠这3个值唯一定位，叫Maven坐标 -->

<dependency>

    <!-- groupId：组织/公司反向域名，相当于大文件夹 -->
    <groupId>junit</groupId>
    
    <!-- artifactId：项目/组件名字，二级文件夹 -->
    <artifactId>junit</artifactId>
    
    <!-- version：版本号，三级文件夹 -->
    <version>3.8.1</version>
    
    <!-- scope：依赖生效范围 -->
    <scope>test</scope>
    
</dependency>
```

本地仓库根目录：`C:\tools\maven_repository`

```
maven_repository/
└── junit/                       ← groupId = junit
    └── junit/                   ← artifactId = junit
        └── 3.8.1/               ← version = 3.8.1
            ├── junit-3.8.1.jar
            ├── junit-3.8.1.pom
            └── 其他校验文件
```

----
**scope**标签
表示依赖范围
1. **compile（默认）** 编译、测试、运行都生效，打包打进最终文件，业务核心依赖用。

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.27</version>
</dependency>
```

2. **test** 仅单元测试代码可用，打包剔除，例 JUnit。

```
<scope>test</scope>
```

3. **provided** 编译需要，服务器运行自带，不打包，例 servlet-api。

```
<scope>provided</scope>
```

4. **runtime** 只运行 / 测试生效，编译不参与，例 JDBC 驱动。

```
<scope>runtime</scope>
```

5. **system** 引用本地磁盘 jar，兼容性差，**禁止使用**。

```xml
<dependency>
    <groupId>com.local</groupId>
    <artifactId>local-sdk</artifactId>
    <version>1.0</version>
    <!-- 固定写 system -->
    <scope>system</scope>
    <!-- system必配：指向本机Jar绝对路径 -->
    <systemPath>D:/lib/xxx-sdk.jar</systemPath>
</dependency>
```

之后点击刷新

***

# 八、添加坐标

1.访问[Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)
搜索要安装的依赖

并复制坐标
例：
```xml
<!-- Source: https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>7.0.8</version>
    <scope>compile</scope>
</dependency>
```

2.粘贴到pom.xml
3.IDEA右侧 m 插件中，点击刷新

# 九、依赖管理（讲解）

### 一、依赖传递

引入 A 依赖，A 自带 B 依赖，Maven 自动把 B 也下载导入项目；优点简化代码，缺点冗余包多，容易版本打架。

### 二、依赖冲突

多条传递路径引入**同一个 jar 不同版本**，JVM 加载混乱，程序报错。
```
                A
               / \
              B   \
             /     \
     spring1.10   spring1.13                          '\'表示依赖
        |              |
     两个版本互相冲突，只能保留一个
        
```

Maven 自动仲裁两大底层规则

1. **最短路径优先** 哪个版本的存放位置距离当前项目层级更近，就选用哪个版本。
2. **先声明优先** 传递路径长度相同时，pom.xml 中先书写的依赖版本生效

### 三、子父工程


```
root-parent/          # 父工程（只管理，不写代码）
 ├── pom.xml          # 父pom：统一锁版本、聚合子模块
 ├── user-module/     # 子模块1（业务模块）
 │   └── pom.xml
 └── order-module/    # 子模块2（业务模块）
     └── pom.xml
```

1. **继承（统一版本）** 父工程锁定依赖版本，**所有子工程不用写版本号**，彻底解决依赖冲突。
2. **聚合（批量构建）** 父工程执行 `install/package`，**所有子模块一次性打包、编译**。

```
                A
               / \
              B   C
             /     \
     spring1.10   spring1.13                '\'表示依赖
        |              |
     两个版本互相冲突，只能保留一个              
        
```

```
                A
               / \
              B   C
              \    \
   spring1.10  └── spring1.13                Maven按Maven 自动仲裁两大底层规则保留了spring1.13

```

**手动解决冲突**
Maven保留了spring1.13
如果想留B的spring1.10



方案 1：在 A 工程 pom 中对 C 依赖直接排除冲突包
修改文件：**A 工程 pom.xml**

```xml
<!-- A依赖B，正常引入 -->
<dependency>
    <groupId>xxx</groupId>
    <artifactId>B</artifactId>
    <version>1.0</version>
</dependency>

<!-- A依赖C，并排除C传递的spring-core 1.13 -->
<dependency>
    <groupId>xxx</groupId>
    <artifactId>C</artifactId>
    <version>1.0</version>
    <exclusions>
    
    
        这是核心代码：仓库中org/springframework/spring-core/下所有依赖将被排除
        
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </exclusion>
        
        
        
    </exclusions>
</dependency>
```


```
                A
               / \
              B   C
             /     \
     spring1.10   （已被排除，无传递）      传到A的只有1.10
```


---

方案 2：在 C 工程内部设置 optional=true（可选依赖）

修改文件：**C 工程 pom.xml**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>1.13</version>
    
    <!-- 标记为可选依赖，上层A不会自动传递继承 -->
    <!-- 这个依赖直供c自己使用 -->
    <optional>true</optional>
    
</dependency>
```

```
                A
               / \
              B   C
             /     \
     spring1.10   （optional=true，不向上传递）
```


### 四、依赖继承

**前提**：不论父模块是否为**compile**，子模块都会继承父模块的依赖

父工程写在 `dependencyManagement` 里的东西，**子模块不会自动继承**。 
子模块想用这个 jar，必须自己在`<dependencies>`手动写上 groupId 和 artifactId

```xml
<!--核心标签-->
<dependencyManagement>                              


  <dependencies>
  
    <dependency>
    
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>5.3.20</version>
      
    </dependency>
    
  </dependencies>
  
<!--核心标签--> 
</dependencyManagement>                               
```

# 十、属性定义

## 2. 在哪定义属性

在 pom 最上方 `properties` 标签里自定义：

```xml
<properties>
    <!-- 自定义一个属性，名字随便起，值写版本 -->
    <spring.version>5.3.20</spring.version>
    <junit.version>4.13.2</junit.version>
</properties>
```

## 3. 用 ${} 调用

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <!-- 直接引用上面定义好的版本号 -->
    <version>${spring.version}</version>
</dependency>
```


# 十一、Nexus仓库

详见 Nexus 私服（Maven 配套工具）