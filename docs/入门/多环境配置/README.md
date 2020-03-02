# 多环境配置

Spring Boot为我们提供了良好的多环境配置方式。

## 多环境配置文件建立

我们需要在resources文件夹下面建立多个环境的配置文件。
以application-{环境变量}.yml为模板

```
|-- resources
    |---- application-local.yml
    |---- application-dev.yml
    |---- application-prod.yml
```

此时如果我们系统中存在环境变量SPRING_PROFILES_ACTIVE或指定spring.profiles.active均可以自动启用相应的配置文件