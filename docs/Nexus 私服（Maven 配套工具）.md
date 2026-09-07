# HELLO！！！
# 欢迎来学Manven

Java语法
git命令
Maven   
Nexus 私服<——目前学到这了欧
spring
mySQL
redis
苍穹外卖
黑马点评


# ！！！！！！和Maven配套，需要先学完Maven！！！！！！
***
# Nexus 简介

Nexus 是程序员专用的**本地软件包仓库服务器**

**仓库**----下载---->**Maven**--------->**项目**

下在jar包时候：
- **中央仓库**-----------------------在海外，下载慢
- **阿里云镜像仓库**----------------下载快，有些自己公司不对外公开的jar包，不能上传阿里云
- **Nexus 私服**--------------------完美解决

平时写 Java 项目需要大量第三方工具包，直接从国外官网下载速度很慢，Nexus 可以充当中转站，代理外网仓库并把下载过的包存在本地，后续重复取用秒级加载，大幅提升开发效率。同时它可以搭建私有存储空间，存放公司内部自己编写的工具 Jar 包，避免内部代码泄露，方便团队所有人统一调用。

很多企业正式服务器禁止连接外网，依靠 Nexus 提前缓存好所有依赖，就能在内网环境完成项目编译、打包，实现离线开发部署。它还能记录所有安装包的上传、下载记录，统一管理版本，避免版本混乱。

Nexus 主要分为三种仓库：
- 代理仓库用来对接外网缓存公共资源；
- 私有宿主仓库分快照库和正式库，分别存放开发测试版本和上线稳定版本；
- 聚合仓库把前面两类合并成一个访问地址，简化项目配置。

除了 Java 的 Jar 包，它还支持前端 npm 包、Docker 镜像、Python 程序包等几乎所有开发文件类型，是软件开发、自动化运维流水线里最常用的基础工具。

***

# 一、下载Nexus
[Download](https://help.sonatype.com/en/download.html)
windows下载<br>[Nexus Repository 3.95.0 for Windows x86-64](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-win-x86_64.zip) ([MD5](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-win-x86_64.zip.md5), [SHA1](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-win-x86_64.zip.sha1), [SHA256](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-win-x86_64.zip.sha256), [SHA512](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-win-x86_64.zip.sha512))
Linux下载
[Nexus Repository 3.95.0 for Linux x86-64](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-linux-x86_64.tar.gz) ([MD5](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-linux-x86_64.tar.gz.md5), [SHA1](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-linux-x86_64.tar.gz.sha1), [SHA256](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-linux-x86_64.tar.gz.sha256), [SHA512](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-linux-x86_64.tar.gz.sha512))
MacOS下载
[Nexus Repository 3.95.0 for Mac Aarch64](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-mac-aarch_64.tar.gz) ([MD5](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-mac-aarch_64.tar.gz.md5), [SHA1](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-mac-aarch_64.tar.gz.sha1), [SHA256](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-mac-aarch_64.tar.gz.sha256), [SHA512](https://download.sonatype.com/nexus/3/nexus-3.95.0-07-mac-aarch_64.tar.gz.sha512))

下载之后把nexus-3.95.0-07-win-x86_64放入tools文件夹内
完整路径
`C:\tools\nexus-3.95.0-07-win-x86_6




# 二、启动Nexus

**Nexus 启动笔记**

##### 1.准备位置

先进入 bin 目录：

```powershell
cd C:\tools\nexus-3.95.0-07-win-x86_64\nexus-3.95.0-07\bin
```

##### 2.首次安装服务

只需要执行一次：

```powershell
.\install-nexus-service.bat
```

##### 3.启动 Nexus

以后每次启动执行：

```powershell
.\nexus.exe start SonatypeNexusRepository
```

##### 4.访问地址

浏览器打开：

http://127.0.0.1:8081

##### 5.首次登录

用户名是 admin。

首次密码在 

`C:\tools\nexus-3.95.0-07-win-x86_64\sonatype-work/nexus3/admin.password

登录后
-  会要求你修改密码。
- admin.password会自动删除

##### 6.停止服务

停止命令：

```powershell
.\nexus.exe stop SonatypeNexusRepository
```


常见说明

- 默认端口是 8081，配置文件在 nexus-default.properties。

- 这套 Nexus 自带 JDK，通常不用额外配置 Java。

- 如果你是在 bin 目录里执行命令，不需要把它加到 Path。

- 首次启动可能要等 1 到 3 分钟。

- 如果网页暂时打不开，先不要急着重启，优先看日志目录 log。


# 三、界面介绍

浏览器打开：

http://127.0.0.1:8081

 左侧是功能导航区：  
## Dashboard

这是总览页。  
***
## Search

这是搜索入口
***
## Upload

这个适合直接把文件传到某个 hosted 仓库里。  
***
## Malware Risk

这是风险和安全相关入口。  
————————————————————————————————————————
## Browse

这是最常用的浏览区。  
可以查看已经创建的仓库，以及仓库里的目录、组件、制品文件。  
如果你上传了 jar、pom、zip、npm 包、docker 镜像等，通常都能在这里看到。

**右侧页面会呈现：

| Name                | Type       | Format | Status                    | URL     |
| ------------------- | ---------- | ------ | ------------------------- | ------- |
| **maven-releases**  | **hosted** | Format | Online                    | 仓库的访问地址 |
| **maven-snapshots** | **proxy**  | maven2 | Online - Ready to Connect |         |
| **maven-public**    | **group**  | nuget  |                           |         |
| maven-central       |            | ....   |                           |         |
| nuget-group         |            | ....   |                           |         |
| nuget-hosted        |            |        |                           |         |

##### Name仓库名字：

- maven-releases  
   一般放正式发布版。

- maven-snapshots  
  一般放开发中的快照版。

- maven-public  
  通常是聚合仓库，给 Maven 客户端统一访问。

- nuget 相关仓库  
   用于管理 .NET / NuGet 包。
   
***
##### Type 仓库类型：

- hosted
  本地托管仓库。  
  一般用来存你们自己上传或发布的包。

- proxy
  远程代理仓库。  
  一般用来代理外部官方仓库，并把下载内容缓存到本地。

- group
  聚合仓库。  
  一般用来把多个仓库合并成一个统一入口，给客户端统一访问。

***
##### Format 仓库格式：

- maven2
  用于管理 Java / Maven / Gradle 相关包。

- nuget
  用于管理 .NET / NuGet 包。

***
##### Status 仓库状态：

- Online
  表示仓库当前在线，可以正常使用。

- Online - Ready to Connect
  表示仓库当前在线，并且已经准备好连接远程仓库。  
  这个状态一般常见于 proxy 仓库。



看到这可能有点晕，下面的流程可以帮助理解：
```md
项目需求  
新建 Nexus 仓库：  
maven-central       （proxy，指向阿里云或中央,一开始是空的，项目要jar包他会从阿里云要）  
maven-releases      （hosted 存非长期版的jar包组合）  
maven-snapshots     （hosted 存非期版的jar包组合）  
maven-public        （group，聚合前三者，相当于文件夹，包含前三者）
			        ↓
Maven settings 镜像只指向 maven-public                          (三、Nexus配)  
	                ↓  
创建 Java Maven 项目（生成 pom）pom编写需要的jar包  
                    ↓  
执行 clean test：先验证下载（空仓会由 proxy 首次拉包并缓存） 
                    ↓  
配置 pom 发布地址 distributionManagement  
                    ↓  
SNAPSHOT 版本 deploy 到 snapshots；正式版 deploy 到 releases    (四、发布快照版正式版)
                    ↓  
后续所有项目统一：下载走 group，上传走 hosted。
```


———————————————————————————————————————


# 三、Nexus配置

##### 1.打开[Browse - Sonatype Nexus Repository](http://localhost:8081/#browse/browse)复制maven-snapshots的url

##### 2、配置setting.xml
`C:\tools\apache-maven-3.9.16\conf\setting.xml

第147行左右，找到`<mirrors>`  
注释掉阿里云，新配置Nexus私服

```XML
  <mirrors>

    <!-- 
    此处为注释
    此处为注释
    此处为注释
     -->

    <!-- <mirror>
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
    </mirror> -->
    <mirror>
      <id>maven-nexus</id>
      <name>Nexus私服</name>
      <url>http://localhost:8081/repository/maven-public</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirors>
```

这段配置是给 Maven 在执行 `mvn deploy` 时提供 Nexus 登录凭证，分别对应正式版和快照版发布仓库的认证。

```xml
  <servers>

    <!-- 
    注释注释注释注释
    注释注释注释注释
    注释注释注释注释
    -->
     <!-- 
    注释注释注释注释
    注释注释注释注释
    注释注释注释注释
    -->

    <server>

      <id>nexus-releases</id>

      <username>admin</username>

      <password>admin</password>

    </server>

    <server>

      <id>nexus-snapshots</id>

      <username>admin</username>

      <password>admin</password>

    </server>

  </servers>
```


# 四、发布快照版 或 正式版

**distributionManagement

`<distributionManagement>` 就是告诉 Maven 在执行 `deploy` 时把正式版发到哪个仓库、把 `SNAPSHOT` 发到哪个仓库，且其 `id` 必须与 `settings.xml` 中 `server.id` 完全一致

## 快照版本

放在 pom.xml 的 project 根节点下
```xml
<distributionManagement>
  <snapshotRepository>
    <id>nexus-snapshots</id>
    <name>Nexus Snapshots</name>
    <url>http://localhost:8081/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```

## 正式版本

放在 pom.xml 的 project 根节点下
```xml
<distributionManagement>
  <repository>
    <id>nexus-releases</id>
    <name>Nexus Releases</name>
    <url>http://localhost:8081/repository/maven-releases/</url>
  </repository>
</distributionManagement>
```
