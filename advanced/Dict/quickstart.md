# 快速开始
## 引入依赖

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-dict</artifactId>
</dependency>
```



## 使用

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


## 开启缓存

默认情况下，字典每次均从数据库中读取，如果您希望其在缓存中存储，则需要开启缓存。

```
app:
    dict:
        cache: true
```

此时的缓存取决于缓存模块的设定。

