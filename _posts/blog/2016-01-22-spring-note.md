---
layout:     post
title:      Spring学习笔记
category:   Java & JavaEE & Spring
description:   记录在学习Spring过程中的一些收获
tags: [java, javaee, Spring]
---

- [Good]
- [Bad]
- [Example]
- [相关资源]

##  [Why]为什么要学习Spring？
从2000年后，**Spring框架是开发JavaEE应用的标配**，几乎我开发过的所有项目使用了他，对于这么重要的框架怎能不去系统的学习一下呢？

## [What]什么是Spring？
在以前的意识中，Spring就是一套框架，提供`DI`、`AOP`、事务管理、其他框架集成等特性，但看一下Spring下面的[项目](http://spring.io/projects)，立刻被震惊到了，现在Spring由19个主项目组成，从框架(Spring Framework)到数据访问(Spring Data)到安全(Spring Security)再到移动端支持(Spring Mobile、Spring for Android)，真是应有尽有。当前**Spring已经发展成为一个平台，叫做`Spring IO Platform`**，旨在：

    Let's build a better Enterprise.
    Spring helps development teams everywhere build simple, portable, fast and flexible JVM-base systems and applications.

看到`Spring IO Platform`这个名字时，不知道你是否会和我产生同样的疑问：Spring输入输出平台？好怪异的名字。经过Google，我发现`IO`理解成`innovation in the Open`(开放中创新)更合适。

下面来一起看看这个平台的全貌吧([官方介绍](http://spring.io/platform/))：

![Spring IO Platform stack](/images/2016-01-22/platform-stack.png)

通过上图，可以看出整个`Spring Platform`分为三大块：

1. 基础(`Foundation`)部分：提供框架、安全、Groovy、反应堆、结构化和非结构化数据的访问、整合、批处理、大数据、web等功能的支持。
2. 运行(`Execution`)部分：为了能快速的使用‘基础部分’提供的支持来构建自己的系统，`Spring IO Platform`特别提供了‘运行部分’的支持。‘执行层’根据不同系统的要求提供了不同的支持，`Spring XD`偏向于管道类系统的构建，`Boot`偏向于基础系统(JavaSE、Web)的构建。
3. 协作(`Coordination`)部分：协作部分提供基于分布式架构的支持，当前的功能开正在不断成熟中。

现在我们对`Spring`已经有了整体上的认识，那我们要使用`Spring IO Platform`中的哪些内容呢？结合当前的需要，我们正在构建一个基于关系型数据库的Web项目，所以我们要使用`基础部分`中的`Framework`、`Relational Data Access`、`web`，以及`运行部分`的`Boot`。

## [How]Spring怎么用？
在Spring的官网中提供了很多的[指南和教程](http://spring.io/guides)，可以通过这些指南快速的了解所需的功能如何构建，当需要更详尽的了解某一个Spring"组件"时，可以进入其对应的[项目](http://spring.io/projects)，通过查看文档来深入了解。

### Spring Boot
`Spring Boot`是

### Spring Framework
`Spring Framework`是

### Spring Data
`Spring Data`是

### Spring Web
`Spring Web`是

## [Nice?]Spring的优缺点
当前对Spring的了解还处在`盲目跟风期`，还不能对Spring做出很多的评价。这个部分的内容，还是待以后完善吧。

## [Resource]相关资源
- [Spring官网](http://spring.io/)
- [Spring Boot参考指南\_中文版](https://qbgbook.gitbooks.io/spring-boot-reference-guide-zh/content/)
- [Spring Framework 4.x 参考指南\_中文版](https://waylau.gitbooks.io/spring-framework-4-reference/content/)
- [深入学习微框架：Spring Boot](http://www.infoq.com/cn/articles/microframeworks1-spring-boot)
- [使用Spring Boot创建微服务](http://www.infoq.com/cn/articles/boot-microservices)
- [Spring Boot性能优化](http://news.oneapm.com/spring-boot/)
- [使用Ratpack与Spring Boot构建高性能JVM微服务](http://www.infoq.com/cn/articles/Ratpack-and-Spring-Boot)
