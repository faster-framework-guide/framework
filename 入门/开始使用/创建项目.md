# 项目创建

首先，您需要通过IDEA创建一个默认的Maven项目

接下来我们需要引入faster框架使用bom管理依赖，当前版本：

[![latest](https://badgen.net/github/release/faster-framework/faster-framework-project?icon=github)](https://github.com/faster-framework/faster-framework-project/releases/latest)

## Maven中的Pom.xml基本配置

pom.xml文件示例如下：

> ps: 此处我们引入了web模块与mybatis模块，以提供默认的接口配置与数据库配置：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cn.org.faster</groupId>
        <artifactId>spring-boot-starters</artifactId>
        <version>${最新版本号}</version>
    </parent>
    <groupId>cn.test</groupId>
    <artifactId>test-project-api</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>cn.org.faster</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>cn.org.faster</groupId>
            <artifactId>spring-boot-starter-mybatis</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
    </build>

</project>
```


> 接下来您需要做一些简单的配置。