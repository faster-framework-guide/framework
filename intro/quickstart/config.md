# 配置文件

Faster的配置非常简单，您只需要加入一个application.yml配置文件便可以快速运行项目。

> yml文件使用yaml语法编写。YAML发音：/ˈjæməl/（yeaimao，建议搜下yutube~)

## application.yml

在resources目录下新建application.yml

引入常用的组件：web、myabtis

```
spring:
  profiles:
    include:
      - web
      - mybatis

```

mybatis组件要求配置数据库连接：

```
app:
  datasource:
    host: 数据库地址
    port: 端口号
    username: 账号
    password: 密码
    name: 数据库名称
```

> 接下来我们将运行项目。