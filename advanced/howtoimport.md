# 如何引入组件

Faster针对常用的模块进行了封装，这些集成模块一旦引入项目，便可以初始化各种基本配置。

一般来说，我们通过以下两个步骤引入这些集成模块：

1. 在maven的pom.xml中

```
        <dependency>
            <groupId>cn.org.faster</groupId>
            <artifactId>spring-boot-starter-mybatis</artifactId>
        </dependency>
```

2. 在SpringBoot的application.yml中

```
spring:
  profiles:
    include:
    - {模块名称}
```


> 接下来我们将一一为您介绍各个模块的集成方式与默认功能。