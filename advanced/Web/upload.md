# 文件上传

文件上传同样是我们应用中一个重要的功能。

Faster内置了图片上传接口，由于图片上传方式众多。

我们只提供接口与默认的本地上传实现。您可以自行实现自己的上传接口。

后续我们会集成一些默认的三方上传服务，如七牛、阿里云oss。

## 快速开始

默认情况下，上传组件是开启的。可以通过app.upload.enabled=false关闭。

### 配置上传路径

您需要配置存储文件的路径以及请求的域名前缀，下面是一个案例

```
app:
  upload:
    local:
      file-dir: /data/upload/faster-framework
      url-prefix: http://127.0.0.1:8080
```

### 创建上传Controller

您需要创建一个controller来实现上传入口：

```
@RestController
public class UploadController extends AbstractUploadController {
}
```

## 客户端使用

AbstractUploadController提供了上传文件所需的接口，客户端需要调用下面两个接口进行上传文件

### 预上传接口

- /upload/preload
- GET
- QueryParam

参数|类型|描述|默认值
---|---|---|---
isCover|int|是否覆盖（0.否 1.是）|1
fileName|string|文件名称|-

- response

```
{
    "sign":"签名",
    "timestamp":"时间戳"
}
```

### 上传文件

- /upload
- POST
- multipart/form-data

参数|类型|描述|默认值
---|---|---|---
file|binary|文件二进制流|
isCover|int|是否覆盖（0.否 1.是）|1
fileName|string|文件名称|-
timestamp|long|时间戳，由预上传接口获取|-
token|string|预上传接口获取|-

- response

```
{
    "url":"文件地址"
}
```


## 预览与下载

### 文件预览

如果您想在浏览器中预览文件，则直接请求文件地址即可。

### 下载文件

如果您想在浏览器中发起下载请求，那您需要在获取到的文件地址后面增加_download端点，即可下载文件


## 自定义上传

### IUploadService

您需要实现IUploadService提供的所有方法。

### application.yml

您需要在配置文件中修改上传模式。

```
app:
  upload:
    enabled: true
    mode: {youUploadMode}
```

### 注册

您需要将组建注册进Spring。

```
    @Configuration
    @ConditionalOnProperty(prefix = "app.upload", name = "mode", havingValue = "{youUploadMode}")
    @ConditionalOnMissingBean(IUploadService.class)
    public static class YourUploadConfiguration {

        @Bean
        public IUploadService uploadService() {
           return new YourUploadService();
        }
    }
```