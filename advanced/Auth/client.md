# 客户端使用

客户端在拿到token后，需要将其放入http请求的header或cookie中，每次请求时携带，key名称如下：

key：Auth-Token

### 获取请求的用户id

您可以通过WebContextFacade获取当前登录信息。

```
WebContextFacade.getRequestContext.getUserId();
```

您也可以使用autowired形式注入RequestContext。我们已经保证了其变量为线程安全的。

```
@Autowired
private RequestContext requestContext;

requestContext.getUserId();

```