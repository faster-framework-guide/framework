# ListWrapper

通常我们会遇到接口的请求参数中存在列表的形式。如果我们希望对列表的数据进行注解验证（@Valided）。我们需要对列表进行方式。

为此我们为您提供了ListWrapper。使用方式如下：

待验证的类中：

```
@Data
public class SysRolePermission {
    private Long roleId;
    @NotNull(message = "请选择权限")
    private Long permissionId;
}
```

controller方法定义中：

```
public ResponseEntity permissions(@Validated @RequestBody ListWrapper<SysRolePermission> list){
}
```
