# 版本控制
在前后端分离、rest接口盛行的当下，接口的版本控制是一个成熟的系统所应该拥有的。
web模块提供的版本控制，可以方便我们快速构建一个基于版本的api接口。

## 快速开始

版本管理默认为开启状态。可以通过faster.version.enabled=fasle关闭。

### 建立包

```
cn.faster.test.modules.v1.user;
cn.faster.test.modules.v2.user;
```

### 建立类

#### V1版本的Controller
cn.faster.test.modules.v1.user.controller.UserController;

```
package cn.faster.test.modules.v1.user;


@RequestMapping("/{version}/user")
@RestController
public class UserController {
    @GetMapping("/extend")
    public ResponseEntity extend() {
        return ResponseEntity.ok("测试可以继承的接口");
    }

    @GetMapping("/hello")
    public ResponseEntity hello() {
        return ResponseEntity.ok("这是版本1的hello接口");
    }
}


```

#### V2版本的Controller

cn.faster.test.modules.v2.user.controller.UserController;

```

@RequestMapping("/{version}/user")
@RestController
public class UserController {
    @GetMapping("/hello")
    public ResponseEntity hello() {
        return ResponseEntity.ok("测试版本2的hello接口");
    }

    @GetMapping("/new")
    public ResponseEntity newApi() {
        return ResponseEntity.ok("测试版本2的新接口");
    }
}

```


### 验证

至此，我们版本1拥有接口：
extend、hello
版本2拥有接口：
extend、hello、new

#### 测试版本1接口


```
http://localhost:8080/v1/extend

输出：测试可以继承的接口
```

```
http://localhost:8080/v1/hello

输出：这是版本1的hello接口
```

#### 测试版本2接口

```
http://localhost:8080/v2/extend

输出：测试可以继承的接口
```

```
http://localhost:8080/v2/hello

输出：测试版本2的hello接口
```


```
http://localhost:8080/v2/new

输出：测试版本2的新接口
```

## 自动解析版本
如果您的分包方式包含版本信息，则将会进行自动版本解析，包名如下：

```
cn.org.faster.test.v1.user;
cn.org.faster.test.v2.user;
```

此时将会自动为您设置接口版本，如上述示例中所示。

您可以通过faster.version.parse-package-version=false关闭此功能。


## @ApiVersion注解

您可以使用注解来标识controller的版本号。如下：

```

@RequestMapping("/{version}/user")
@RestController
@ApiVersion(2)
public class UserController {
    @GetMapping("/hello")
    public ResponseEntity hello() {
        return ResponseEntity.ok("测试版本2的hello接口");
    }

    @GetMapping("/new")
    public ResponseEntity newApi() {
        return ResponseEntity.ok("测试版本2的新接口");
    }
}
```

配置优先级：@ApiVersion > parse-package-version

## 版本废弃

您可以使用faster.version.minimum-version配置来设置当前系统中允许的最小版本，以此废弃该版本之前的所有版本。如：

```
faster:
    version:
        minimum-version: 5
```

此时5版本之前的版本请求时均会返回：

```
{"timestamp":"1546956010916","status":"0","code":"1005","message":"当前版本已停用，请升级到最新版本。"}
```

当然还可以使用@ApiVersion(discard=true)注解来废弃版本。

注意：如果parse-package-version=true，此时将需要使用@ApiVersion(discard=true, overrideDiscard = true)来使得注解生效。
