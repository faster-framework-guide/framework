# 请求上下文

每个项目中总有一些公用的业务信息，我们希望快速获取并且无须在每个接口的参数中定义，方便管理与维护。

上下文始于request，终于response。

## 快速开始

框架提供了两种获取业务上下文的方式。

### 注入使用

```
@Autowired
private RequestContext requestContext;
```

### 静态获取


```
public void test(){
    WebContextFacade.getRequestContext().getUserId();
}
```


## 扩展字段

除了一些常用字段做了封装外，在真实场景下我们可能还会遇到一些其他信息，为此我们将extra加入了上下文对象中。您可以这样使用：


### Map形式

```
# 设置
requestContext.extraMap("key","value");
WebContextFacade.setRequestContext(requestContext);
# 获取
RequestContext requestContext = WebContextFacade.getRequestContext();
Object value = requestContext.extraMap("key");
```

### Bean形式

```
# 设置
requestContext.setExtraBean(user);
WebContextFacade.setRequestContext(requestContext);
# 获取
RequestContext requestContext = WebContextFacade.getRequestContext();
User user =  requestContext.getExtraBean(new TypeReference<User>(){});
```

## 注意事项

需要注意的是，由于使用ThreadLocal实现上下文的获取，生命周期只存在于当前请求的线程中（controller所在线程）。所以如果希望在其他线程中使用，则需要先在当前线程中获取数据，并传递到其他线程中使用。