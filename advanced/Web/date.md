# 日期

我们推荐使用Java8的日期组件，但很多三方插件都需要额外的包才可以支持LocalDate及LocalDateTime。


## 请求参数

前端传递 ``` yyy-MM-dd hh:mm:ss ```  格式或 ``` yyy-mm-dd ``` 格式的字符串即可，系统会自动转换为相应格式，无须定义注解。

```
@Data
public class Test{
 private LocalDateTime date;
 private LocalDate date2;
}
```

## 响应参数

默认将LocalDatetime类型的属性映射为 ``` yyy-MM-dd hh:mm:ss ``` 字符串。
默认将LocalDate类型的属性映射为 ``` yyy-MM-dd ``` 字符串。
