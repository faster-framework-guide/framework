# Redis缓存

如果您需要将缓存存入redis，您需要做以下几个步骤

## 引入依赖


```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
</dependency>
```

## 配置

```
spring:
  profiles:
    include:
      - redis
app:
  redis:
    host: 地址
    password: 密码
```

## 使用

### CacheFacade

CacheFacade用于暴露简单的key/value形式的缓存操作，由yml配置文件控制缓存中间件的使用。
在CacheFacade的高级抽象中缓存的value值使用fastjson进行序列化，对于任意的缓存中间件，实际存储的是json字符串。
具体操作可参见上一节的基础使用。

### RedisTemplate

如果我们希望使用redisTemplate进行操作redis，以实现更加复杂的数据结构。
框架提供了RedisHelper来获取template，可以传入泛型类，来支持集合类型的泛型结构。

```
RedisHelper.template(T.class);
```
