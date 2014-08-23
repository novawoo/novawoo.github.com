---
layout:     post
title:      如何使用Maven
category:   blog
description: 使用Maven的一些记录
tags:
 - 记录
---
## 1. 简介
Maven是基于项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。
Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，许多 Apache Jakarta 项目现在使用 Maven，而且公司项目采用 Maven 的比例在持续增长。
![principle](../../images/2014-08-17-how-to-use-maven/maven-dependencies.jpg)

## 2. 安装
Maven是一个Java工具，所以在安装Maven之前，要确保已经安装了JDK。

1. [下载Maven](http://maven.apache.org/download.cgi "下载")
2. 解压下载下来的文件到自己经常安装软件的目录，eg：e:/util/maven/apache-maven-3.2.2
3. 配置环境变量，与配置JDK的方法一样  
   新建一个变量：`M2_HOME`，将其路径设置为第2步中的路径  
   在`Path`环境变量中增加，`%M2_HOME%\bin`的配置 
4. 测试是否安装成功  
   在命令行下，输入`mvn --version`，出现下图即为成功
   ![sucess](/images/2014-08-17-how-to-use-maven/maven-install-sucess.png)
5. 复制setting.xml到用户目录下，方便自定义设置，方法：将`${M2_HOME}/conf/settings.xml` 复制到 `${user.home}/.m2/settings.xml`

## 3. 要认识的目录
- 主配置文件：`${user.home}/.m2/settings.xml` [介绍](http://blog.csdn.net/tomato__/article/details/13025187 "介绍")
- 本地仓库目录:`${user.home}/.m2/repository/`
- 超级POM：`${M2_HOME}/lib/maven-model-builder-x.x.x.jar` `\org\apache\maven\model\pom-4.0.0.xml`

## 4. 要掌握的概念

### 4.1 POM
项目对象模型（Project Object Model，POM）描述项目的各个方面，每个Maven项目都应该有一个`pom.xml`文件。

通常，`pom.xml`文件由三个主要部分组成([官方介绍](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html "官方介绍"))：

- 项目管理部分包括项目的组织、开发人员名单、源代码位置和错误跟踪系统 URL 等信息。
- 项目相关性部分包括关于项目相关性的信息。
- 项目构建和报告部分包含项目构建信息（如源代码目录、单元测试用例目录）和要在构建中生成的报告。  

![pom说明](/images/2014-08-17-how-to-use-maven/maven-pom.PNG)

### 4.2 坐标

#### 4.2.1 坐标介绍
坐标为构件（各种jar包）引入了秩序，世界上任何一个构件都可以使用Maven坐标唯一标识，Maven坐标的元素包括`groupId` `artifactId` `version` `packaging` `classifier`，我们在开发项目的时候，也需要定义一个坐标，这是Maven强制要求的。--[官方对坐标的介绍](http://maven.apache.org/pom.html#Maven_Coordinates "官方对坐标的介绍")

下面借助一个例子来介绍坐标中的各个元素：

    
    <groupId>com.mycompany.app</groupId>  
    <artifactId>app_moduleName</artifactId>  
    <packaging>jar</packaging>  
    <version>0.0.1-SNAPSHOT</version>  
    
- groupId ：定义当前Maven项目隶属的实际项目
- artifactId : 定义当前实际项目中的一个Maven项目（模块）
- version : 该元素定义Maven项目当前的版本
- packaging ：定义Maven项目打包的方式(不定义时，默认为：jar)，也可以打包成war, ear等
- classifier: 该元素用来帮助定义构建输出的一些附件（如：javadoc、sources等），注意，不能直接定义项目的classfier，因为附属构件不是项目直接默认生成的，而是由附加的插件（常用Maven Assembly Plugin）帮助完成。


#### 4.2.2 坐标最佳实践
- groupId的值一般为：com.company.projectName
- artifactId的值一般为：projectName_moduleName
- version的值一般为：<主版本>.<次版本>.<增量版本>-<限定符>，其中主版本主要表示大型架构变更，次版本主要表示特性的增加，增量版本主要服务于bug修复，而限定符如snapshot、alpha、beta等等是用来表示里程碑。

![版本控制](/images/2014-08-17-how-to-use-maven/maven-scm.jpg)

### 4.3 依赖
依赖顾名思义，就是这个项目对其他构建或工程的依赖，配置了依赖就相当于将相应的jar包放到了lib目录下（当然这是狭义的理解）。
    
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
根元素project下的dependencies可以包含也或多个denpendency元素，用来声明一个或者多个依赖，每个依赖可以包含的元素有([官方对依赖的介绍](http://maven.apache.org/pom.html#Dependencies))：

- groupId、artifactId、version:依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven根据坐标才能找到依赖。
- type:依赖的类型，对于项目坐标定义的packaging。大部分情况下，该元素不必声明，默认为jar。
- scope：依赖范围: compile、test、provided、runtime、system
- optional：标记依赖是否可选 true|fals
- exclusions：用来排除传递性依赖
大部分依赖声明只包含基本坐标，然而在一些特殊的情况下，其他元素至关重要。

### 4.4 仓库
Maven仓库是用来帮助我们存储和管理公共构件（主要是jar包）的地方。在Maven世界中，任何一个依赖、插件或者项目构建的输出，都可以成为构件。

实际上Maven项目不会各自存储其依赖文件，它们只需要声明这些依赖的坐标，在需要的时候（例如：编译项目的时候需要将依赖加入到classpath中），Maven会自动根据坐标找到仓库中的构件，并使用它们。

为了实现重用，项目构建完毕后生成的构件也可以安装或者部署到仓库中，供其他项目使用。

运行Maven的时候，Maven所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

#### 4.4.1 本地仓库
Maven安装完成后，会在本地创建一个`repository`目录，这个目录即为本地仓库。本地仓库的默认路径为：${user.home}/.m2/repository/，也可以自定义本地仓库的位置，修改${user.home}/.m2/settings.xml ：

    
    <settings>
      ...
      <localRepository>E:\java\repository</localRepository>
      ...
    </settings>
    

- 在执行`mvn install`命令时，Maven会将项目生成的构建安装到**本地仓库**，供**本地**其他构建使用。
- 在执行`mvn deploy`命令时，Maven会将项目生成的构建发布到**远程仓库**，供**所有能访问该仓库的用户**使用。

#### 4.4.2 远程仓库
原始的Maven安装自带了一个远程仓库——Maven中央仓库，中央仓库的id为central，远程url地址为http://repo1.maven.org/maven2，它关闭了snapshot版本构件下载的支持。

#### 4.4.3 特殊的远程仓库--Maven私服
通过建立私服，可以降低中央仓库负荷、节省外网带宽、加速Maven构建、自己部署构件等，从而高效地使用Maven。
![私服的作用](/images/2014-08-17-how-to-use-maven/maven-nexus.png)


### 4.5 生命周期和插件
Maven对构建(build)的过程进行了抽象和定义，这个过程被称为构建的生命周期(lifecycle)，Maven的生命周期是抽象的，这意味着声明周期本身不做任何实际的工作，在Maven的设计中，实际的任务（如编译源代码）都交由插件来完成。

Maven有三套相互独立的生命周期，而且“相互独立”，这三套生命周期分别是([参考](http://juvenshun.iteye.com/blog/213959 "参考"))：

- Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
- Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。
- Site Lifecycle 生成项目报告，站点，发布站点。

![lifecycle-clean](/images/2014-08-17-how-to-use-maven/maven-lifecycle.gif)

而每套生命周期都是一组**阶段(Phase)**组成，各套Lifecycle 的Phase如下：

- Clean Lifecycle
  1. pre-clean  执行一些需要在clean之前完成的工作；
  2. clean  移除所有上一次构建生成的文件；
  3. post-clean  执行一些需要在clean之后立刻完成的工作；

- Site Lifecycle
  1. pre-site  执行一些需要在生成站点文档之前完成的工作；
  2. site  生成项目的站点文档；
  3. post-site  执行一些需要在生成站点文档之后完成的工作，并且为部署做准备；
  4. site-deploy  将生成的站点文档部署到特定的服务器上；

- Default Lifecycle
  1. validate
  2. initialize
  3. generate-sources
  4. process-sources
  5. generate-resources
  6. process-resources  复制并处理资源文件，至目标目录，准备打包；
  7. compile  编译项目的源代码；
  8. process-classes
  9. generate-test-sources 
  10. process-test-sources 
  11. generate-test-resources
  12. process-test-resources  复制并处理资源文件，至目标测试目录；
  13. test-compile  编译测试源代码；
  14. process-test-classes
  15. test  使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署；
  16. prepare-package
  17. package  接受编译好的代码，打包成可发布的格式，如 JAR ；
  18. pre-integration-test
  19. integration-test
  20. post-integration-test
  21. verify
  22. install  将包安装至本地仓库，以让其它项目依赖；
  23. deploy  将最终的包复制到远程的仓库，以让其它开发人员与项目共享；

这些阶段(Phase)是有顺序的，并且后面的阶段依赖于前面的阶段，用户和Maven最直接的交互方式就是这些生命周期阶段（eg：mvn clean install）。

![lifecyle](/images/2014-08-17-how-to-use-maven/maven-lifecyle.gif)

### 4.6 聚合与继承
简单的解释：

- 聚合：一个pom包含多个pom
- 继承：一个pom中的内容被多个pom使用，与Java中的继承类似

### 4.7 约定优于配置

#### 4.7.1 实现
约定优于配置的实现是通过超级POM实现的，所有的pom文件都继承自超级POM，在超级POM中定义很多约定。

#### 4.7.2 好处
Maven提倡使用一个共同的标准目录结构，使开发人员能在熟悉了一个Maven工程后，对其他的Maven工程也能清晰了解。这样做也省去了很多设置的麻烦。

#### 4.7.3 Maven标准的目录结构
Maven标准工程的根目录下就只有src和target两个目录，各文件及目录的定义如下：
    
    src `存放项目的源文件`
      -main
          –bin `脚本库`
          –java `java源代码文件`
          –resources `资源库，会自动复制到classes目录里`
          –filters `资源过滤文件`
          –assembly `组件的描述配置（如何打包）`
          –config `配置文件`
          –webapp `web应用的目录,WEB-INF、css、js等`
      -test
          –java `单元测试java源代码文件`
          –resources `测试需要用的资源库`
          –filters `测试资源过滤库`
      -site `Site（一些文档）`
    target `存放项目构建后的文件和目录，jar包、war包、编译的class文件等`
    LICENSE.txt `工程的license文件`
    README.txt `工程的readme文件`
    

