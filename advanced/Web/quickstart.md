# 快速开始

如果您想快速构建一个不包含mybatis的web项目，那么您只需要以下三个步骤。

## 引入POM依赖

在pom.xml中引入以下组件。

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 引入配置文件

您需要在resources目录下建立application.yml文件，并且引入以下配置。

```
spring:
  profiles:
    include:
      - web
```

## 启动类建立

在某个包下建立SpringBoot的启动类并且运行。

```
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

> 接下来的我们将为您介绍Web模块下的内置组件。