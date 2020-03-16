# 错误处理

我们这里所介绍的异常处理是指程序中不可预知的运行时异常。



## 全局处理

我们已经内置了全局异常拦截器，在您的代码中抛出异常时，我们将捕获异常并返回异常信息。

返回异常分两种：

### 接口异常

当请求目标为接口时，抛出异常将返回json格式的内容响应，并且http状态码均为400以上。这里的接口指的是包含RestController注解或ResponseBody注解。

返回json格式如下：

```
{
    "timestamp":1499404172029,//请求时间戳
    "status":400,//http状态码
    "path":"/v1/version",//请求路径
    "code":"2",//业务状态码
    "message":"nickname不能为空"//错误信息
}
```

### 页面异常

当请求目标为页面模板时，抛出异常将转向error路由，您可以自定义自己的错误页面。定义错误页面需要您建立Controller并且实现ErrorController接口，如下：

```
@Controller
@RequestMapping
public class CustomErrorController implements ErrorController {

    @GetMapping("/error")
    public String error(Model model) {
        return "/error.ftl";
    }

    @Override
    public String getErrorPath() {
        return "/error";
    }
}
```


## 错误码定义

我们希望您使用错误码对接口的错误逻辑进行处理。

为此我们提供了ErrorCode接口。您需要自定义自己的业务逻辑异常，并实现此接口。

```
@AllArgsConstructor
@Getter
public enum SysPermissionError implements ErrorCode {
    PERMISSION_PARENT_NOT_EXIST(1110, "权限父级不存在"),
    PERMISSION_CODE_EXIST(1111, "权限编码已经存在");
    private int value;
    private String description;
}
```

### 错误码范围

系统默认使用小于2000的状态码作为系统异常码。

我们希望您使用2000以上的错误码作为自己的业务逻辑错误码。

## 异常的抛出


您可以在接口中通过我们提供的ResponseErrorEntity.error创建返回的异常实体。

```
return ResponseErrorEntity.error(SysPermissionError.PERMISSION_CODE_EXIST, HttpStatus.BAD_REQUEST);
```

或

```
return ResponseErrorEntity.error("错误信息", HttpStatus.BAD_REQUEST);
```

或抛出异常

```
SysPermissionError.PERMISSION_CODE_EXIST.throwException();
```

## http错误码

通常在错误情况下，您需要使用400以上的http状态码。常用http状态码如下：

Code|状态|描述
---|---|---
200|OK |[GET]：服务器成功返回用户请求的数据。
201|CREATED |[POST/PUT/PATCH]：用户新建或修改数据成功。
202 |Accepted| [*]：表示一个请求已经进入后台排队（异步任务)
204 | NO CONTENT | [DELETE]：用户删除数据成功
400 | INVALID REQUEST |[POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作。
401 |Unauthorized|[*]：表示用户没有权限（令牌、用户名、密码错误）。
403 |Forbidden|[*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 |NOT FOUND|[*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作。
406 |Not Acceptable|[GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
410 |Gone|[GET]：用户请求的资源被永久删除，且不会再得到的。
422 |Unprocesable entity|[POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
500 |INTERNAL SERVER ERROR|[*]：服务器发生错误，用户将无法判断发出的请求是否成功。