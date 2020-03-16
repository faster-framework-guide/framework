# 图形验证码

Faster的图片验证码规则如下：

1. 前端请求图形验证码接口，生成base64图片字符串与token，返回给前端使用。

2. 前端通过base64展示图片，提交时携带token。

3. 通过图形验证码服务接口验证token与用户填写的验证码是否相同。

4. 图形验证码有效期为1分钟。


## 快速开始

### 创建Controller

您需要创建一个Controller来暴露图形验证码接口。

```
@RestController
public class CaptchaController extends AbstractCaptchaController {
}
```

## 客户端使用

AbstractCaptchaController提供图形验证码所需的接口，客户端需要调用下面两个接口进行上传文件

### 获取验证码

- /captcha
- GET
- response

```
{
    "img":"base64编码",
    "token":"验证凭据"
}
```

### 验证图形验证码

- /captcha/valid
- GET
- QueryParam

参数|类型|描述|默认值
---|---|---|---
captcha|string|用户输入的图形验证码内容|-
token|string|凭据|-


## 手动验证

我们还可以使用ICaptchaService提供的验证接口来验证用户的验证码是否有效。


```
@Autowired
private ICaptchaService captchaService;

boolean valid = captchaService.validCaptcha(inputCaptcha, captchaToken);
```