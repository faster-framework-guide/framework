# 响应消息体

Faster在返回消息体时，使用MVC的ResponseEntity进行包装，接口的响应数据直接作为消息体返回给前端，无须包装额外的code与message属性。

```
ResponseEntity.ok(list);
```

Faster扩展了ResponseEntity，为您提供了ResponseErrorEntity。您可以配合ErrorCode返回直观的错误信息。

您可以参考异常处理一节，来探寻如何抛出异常。