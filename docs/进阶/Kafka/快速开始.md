# 快速开始

## 引入

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-kafka</artifactId>
</dependency>
```

## 配置

application.yml文件中引入kafka配置：

```
spring:
  profiles:
    include:
    - web
    - kafka
```


### 全局开关

可以通过faster.kafka.enabled配置控制kafka模块组件的开关

```
app:
    kafka:
        enabled: true #默认为true
```
