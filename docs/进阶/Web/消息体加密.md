# 消息体加密
在一些安全性要求较高的项目中，我们希望前端请求的数据可以做到数据加密，服务器端解密使用。（单纯的HTTPS仍难以满足安全需要。）

我们提供了secret模块针对消息体进行加密，目前仅支持请求消息加密。（响应消息过大情况下，加密会带来严重的性能问题。）

流程如下：
我们使用DES-CBC算法对称加密请求体。客户端请求前加对消息体进行加密，服务器端通过SpringMVC Advice拦截请求解密后，传给controller的方法。

secret模块针对加密的json请求体进行验证，并将密文解密为明文后传递给controller层，整个过程对开发人员透明。

## 1.1 使用

### 1.1.1 密钥、向量配置

secret目前使用DES-CBC算法进行解密。故请求体也应为DES-CBC算法进行加密。
密钥与向量默认如下：

```
app:
  secret:
    enabled: true
    des-secret-key: b2c17b46e2b1415392aab5a82869856c #默认值，设置自己密钥时修改
    des-iv: 61960842 #默认值，设置自己密钥时修改
```

### 1.1.2 全局扫描

默认情况下，在yml文件中启用secret配置即开启全局扫描模式。

application.yml:

```
app:
  secret:
    enabled: true
```

此模式会对当前项目中的所有请求进行拦截。

### 1.1.3 包扫描

可以通过scan-packages属性配置仅针对哪些包起效。支持通配符，支持多路径。

application.yml:

```
app:
  secret:
    enabled: true
    scan-packages: cn.test.**.consumer,cn.test.admin.user.web
```

### 1.1.3.1 包排除

可以通过exclude-packages属性配置排除哪些包。支持通配符，支持多路径。

application.yml:

```
app:
  secret:
    enabled: true
    exclude-packages: cn.test.**.admin,cn.test.**.consumer.user.web
```

### 1.1.4 注解扫描

可以通过开启scan-annotation配置及使用@SecretBody注解，配置某个类或方法级别的拦截。

注意：此模式下，只有标识了注解的类或方法，才会进行拦截。

application.yml:

```
app:
  secret:
    enabled: true
    scan-annotation: true
```

#### 1.1.4.1 类级别

TestController.class

```
/**
 * 该类全部方法均会被拦截
 * 
 */
@RestController
@RequestMapping("/test")
@SecretBody
public class TestController {

    /**
     * 测试加密请求体
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret")
    public void secret(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }

    /**
     * 测试加密请求体
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret2")
    public void secret2(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }
}

```

#### 1.1.4.2 方法级别

TestController.class

```
/**
 * 
 * 仅标识的方法被拦截
 */
@RestController
@RequestMapping("/test")
public class TestController {

    /**
     * 测试加密请求体
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret")
    @SecretBody
    public void secret(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }

    /**
     * 测试加密请求体
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret2")
    public void secret2(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }
}

```

#### 1.1.4.3 注解扫描排除

@SecretBody提供exclude配置可排除当前所标识的方法或类。

```
@RestController
@RequestMapping("/test")
@SecretBody
public class TestController {

    /**
     * 测试加密请求体，即使类含有@SecretBody，方法级别标识exclude后，扔不会拦截。
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret")
    @SecretBody(exclude = true)
    public void secret(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }
    
    /**
     * 测试加密请求体，类标识了@SecretBody，方法没有标识exclude，会被拦截
     *
     * @param secretBean
     * @return
     */
    @PostMapping("/secret2")
    public void secret2(@RequestBody SecretBean secretBean) {
        log.info("请求数据:{}", secretBean);
    }
}
```