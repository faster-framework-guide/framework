# 快速开始

## 引入依赖

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-sms</artifactId>
</dependency>
```

## 引入配置

您需要选择您想要使用的短信平台。以阿里云短信功能为例。您需要在yml文件中启用以下配置：

```
spring:
  profiles:
    include:
      - sms
app:
  sms:
    captcha:
        expire: 60 //超时时间（s），默认15分钟
```


## 使用

当您需要使用短信功能时，只需注入所引入的服务即可，其中泛型为相应服务所需参数，继承ISmsService时指定。

```
    @Autowired
    private ISmsService<?> smsService;

    smsService.send(?);
```

默认情况下，我们已经集成了阿里云的短信服务，您需要在application.yml中配置以下内容：

```
app:
    sms:
        ali: 
            accessKeyId: 1
            accessKeySecret: 2
```

## 配置列表

多版本组件的所以配置均在app.sms节点下。


属性|说明|类型|默认值
---|---|---|---
enabled|是否开启|boolean |true
debug|是否为debug模式，填入任何短信验证码均可成功|boolean|开发环境默认开启
mode|模式|string|ali
ali.accessKeyId|阿里短信验证码id|string|-
ali.accessKeySecret|阿里短信验证码密钥|string|-
captcha.expire|超时时间（秒）|long|900(15分钟)