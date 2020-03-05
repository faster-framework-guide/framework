# 配置

默认情况下，字典每次均从数据库中读取，如果您希望其在缓存中存储，则需要开启缓存。

```
app:
    dict:
        cache: true
```

此时的缓存取决于缓存模块的设定。

# 字典结构

字典实体定义如下：

```
@Data
@EqualsAndHashCode(callSuper = true)
@TableName("sys_dict")
public class SysDict extends BaseEntity {
    /**
     * 字典key
     */
    private String name;
    /**
     * 类型
     */
    private String type;
    /**
     * 字典值
     */
    private String dictValue;
    /**
     * 展示状态（0.不展示1.展示）
     */
    private Integer showStatus;
}
```

# DictFacade

我们提供了DictFacade门面类来供大家快速调用。

全部字典：

```
List<SysDict> list = DictFacade.listAll()
```

根据主键id获取字典：

```
SysDict dict = DictFacade.byId(id)
```

根据类型、名称获取字典

```
SysDict dict = DictFacade.byTypeAndName(type,name)
```

根据类型获取字典列表

```
List<SysDict> list = DictFacade.byType(type)
```
