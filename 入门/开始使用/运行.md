# 运行

## 建立包名

由于Faster基于Spring Boot创建，要求必须提供包名。

此处我们建立cn.test这个包

## 建立入口类

在cn.test下建立TestApplication.java，具体代码如下：

```
package cn.test;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TestApplication {
    public static void main(String[] args) {
        SpringApplication.run(TestApplication.class);
    }
    @RestController
    @RequestMapping("/test")
    public static class TestController {
        @GetMapping("/hello")
        public ResponseEntity hello(){
            return ResponseEntity.ok("hello faster");
        }
    }
}
```

## 运行并验证

1. 在main方法上右键run。

2. 等待项目启动直到终端输出Tomcat started on port(s): 8080 (http) with context path说明运行成功。

3. 请求 http://localhost:8080/test/hello 进行验证是否成功。


> 接下来的步骤便于SpringBoot+Mybatis开发的方式一样，您可以按照包来分类，并且建立自己的Controller、Service、Mapper。