# 快速开始

## 引入POM依赖

在pom.xml中引入以下组件。

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-auth</artifactId>
</dependency>
```


## 生成token

您需要注入AuthService，传入需要授权的目标以及失效时间。

授权目标为字符串，一般可传入用户id。
失效时间单位为秒。

```
@Autowired
private AuthService authService;

public String token(){
    authService.createToken(audience,expSecond);
}
```

## 标注验证

您可以在需要登录的接口controller或方法上加入@Login注解。


