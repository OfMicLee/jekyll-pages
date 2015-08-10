---
layout: post
title: "spring-boot快速开发spring应用"
description: "使用spring-boot快速创建和运行一个spring 应用"
category: "other"
tags: [spring, spring-boot]
---

##Spring自动化框架

spring多年以来一直都是java平台开发web应用的主流技术，在标准的J2EE架构之外提供了一个轻量级的解决方案。虽然spring提供了很多功能，简化了java平台的企业应用开发，降低了开发工作量，但相比较其它语言的一些框架（例如ruby on rails，python Django）来说，基于spring 的开发仍然比较复杂，尤其是新建一个项目时，需要进行各种配置，重复的工作量较大。

针对这个问题spring开源社区一直都在持续不断地进行探索，提供相应的解决方案。

- grails  

  使用groovy语言，封装了spring，提供了一个高效的全栈框架，在开发效率方面可媲美ROR，但运行时性能比ROR要高很多。grails用户群较大，目前开发社区比较活跃。

- spring roo

  在spring之上提供一个纯java的封装，使用到了osgi，aspectj等技术，提供类似ROR的开发模式，但未成功推广，用户不多，目前开发基本停滞。

- spring-boot

  spring-boot 是spring社区今年推出的一个新项目，其主要目的也是提高生产率，尤其是快速创建和运行一个spring 应用。


##spring-boot

###spring-boot功能特性

- 创建独立运行的spring应用

  使用spring-boot，可将整个spring应用打包为一个独立的jar文件，内嵌tomcat或者jetty容器。通过 java -jar xxx.jar 即可运行，免去了部署到应用服务器的步骤。

- 启动器

  针对典型的应用需求，提供了一些标准的启动器配置，声明对这些启动器的依赖即可获得相关功能。例如如果需要使用jpa进行数据访问，仅需加入

  ```
  ${project.groupId}
  spring-boot-starter-data-jpa
  ```
  spring-boot会自动配置通过jpa进行数据访问需要的bean。  

- 自动配置spring

  spring-boot会根据classpath包含的内容自动推测用户的需求并自动配置。例如如果在classpath包含了hsqldb，并且用户未配置数据库连接，spring-boot将会配置一个hsqldb内存数据库和数据源。

- 自动生成生产环境需要的特性

  spring-boot能够为应用自动加入一些典型的生产环境下的功能特性，例如：外部配置，安全，日志，管理，审计等。

- 无代码生成，无xml配置需求

  spring-boot无代码生成，所有的配置可通过代码完成（spring 的javaconfig），不需要使用xml（虽然可以使用）。


###spring-boot组件

spring-boot项目分为几个不同的组件，下面是每个组件的说明：

- spring-boot SpringBootApplication

提供静态方法，用于开发独立运行的应用
嵌入容器配置，tomcat或者jetty
外部配置支持，从命令行，属性文件读取配置
spring context 的初始化  

- spring-boot-autoconfigure

自动配置框架：根据classpath推测用户需要的功能并自动配置。例如如果在classpath 包含了  HSQLDB，并且没有配置数据库连接，spring-boot-autoconfig将自动配置一个内存数据库。spring-boot-autoconfigure使用spring 的javaconfig功能，为一个 @Configuration 类加上 @Conditional注解，注解声明的条件满足时,配置就会生效。用户可编写自己的配置类对自动配置进行扩展。

- spring-boot-starters

一组预定义的依赖，添加不同类型的应用功能。例如如果需要jpa数据访问，加入 spring-boot-starter-data-jpa

- spring-boot cli

一个命令行工具，可以直接运行一个groovy脚本作为spring 应用。例如以下groovy代码定义
了一个spring mvc controller，通过运行  spring run HelloController.groovy 即可运行一个spring web 应用。

```java
@Controller
class HelloController {
    @RequestMapping("/")
    @ResponseBody
    String home() {
        return "Hello World!"
    }
}
```

groovy语言语法非常类似java语言，大部分代码都可以直接拷贝使用。spring-boot cli提供的功能非常适用于快速原型开发，以及在开发环境中搭建测试/模拟服务器等。（今年一个国人开发的开源项目 moco获得了“ duke选择奖”，moco的主要功能就是搭建测试/模拟服务器，借助于spring-boot cli，只需要用java语言，几分钟之内也可以搭建一个测试/模拟服务器）  

- spring-boot-actuator

提供更多面向生产环境的支持，安全，日志，管理，审计。

- spring-boot-loader

使用java -jar xxx.jar 运行应用的实现，对打包文件格式进行了定义。一般通过gradle插件或者 maven插件使用。这两个插件提供了在gradle和maven构建系统中使用spring-boot的功能。使用你的IDE创建一个普通的java 项目，加入这两个插件即可在项目中引入spring-boot。下面是使用java语言开发的controller：

```java
package hello;

import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@Controller
@EnableAutoConfiguration
public class SampleController {

    @RequestMapping("/")
    @ResponseBody
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(SampleController.class, args);
    }
}
```

相比较groovy而言，java语言开发的代码中需要包含一个 main 静态执行入口，在其中调用 SpringApplication.run 方法启动spring 应用。
对于包含上述代码的项目使用gradle和maven插件，可以将spring应用打包成为一个单独的jar文件，然后使用
java -jar xxxx.jar 执行。
