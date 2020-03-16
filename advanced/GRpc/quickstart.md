# 快速开始

Faster引入了Google的GRpc模块，本篇文章带您快速使用。

参阅本教程前我们希望您已经阅读过基本的grpc使用方式。如果您尚不熟悉，请移步grpc官方查看。

[grpc-java](https://grpc.io/docs/tutorials/basic/java.html)


## 引入依赖

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-grpc</artifactId>
</dependency>
```

## 服务端

### 配置

```
faster:
  grpc:
    enabled: true
    server:
      enabled: true #实际可不写，默认开启
      port: 50051 #端口，不写默认50051
```

### 编写接口

@GRpcApi中的值为scheme，客户端的@GRpcService(scheme="")与服务端相同时，即可定位到服务端的服务。

@GRpcMethod默认获取当前方法名称，客户端同样使用此注解标识要调用的服务方法，可通过@GRpcMethod("xxx")修改名称，两者一致调用成功。

注意：对于GRpc而言，请求参数中只能出现一个业务参数实体，才可通过序列化工具进行序列化，不可出现多个业务参数。

```
@GRpcApi("unary")
@Slf4j
public class UnaryService {

    @GRpcMethod
    public void testInt(int data, StreamObserver<Integer> response) {
        log.info("请求数据:{}", data);
        response.onNext(1);
        response.onCompleted();
    }
}
```


## 客户端

### 配置文件

在application.yml配置如下参数：

```
app:
  grpc:
    enabled: true #实际可不写，默认开启
    client:
      enabled: true #实际可不写，默认开启
      services:
        test-unary: #与GRpcService中的value对应
          host: localhost # 远程主机地址
          port: 50051 # 远程端口号，不写默认50051
        test-server-stream: #与GRpcService中的value对应
          host: localhost #服务端host
```

### 定义远程接口

@GRpcService中的value对应配置文件中services下的key，以此获取远程host与端口。

@GRpcService中的scheme对应远程服务@GRpcApi("unary")中所定义的值，用于创建grpc fullMethod，两者一致，即可调用成功。

@GRpcMethod默认获取当前方法名称，远程服务端同样使用此注解标识提供服务的方法，可通过@GRpcMethod("xxx")修改名称，两者一致调用成功。

注意：对于GRpc而言，接口参数只能存在一个业务实体参数，才可通过序列化工具进行序列化，不可出现多个业务参数。


```
@GRpcService(value = "test-unary", scheme = "unary")
public interface UnaryService {
    @GRpcMethod
    int testInt(int data);
}
```

### 调用

```
@Slf4j
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
public class UnaryTest{
    @Autowired
    private UnaryService unaryService;

    @Test
    public void testInt() {
        int result = unaryService.testInt(1);
        log.info("返回数据:{}", result);
    }
}
```



## 更多方式

GRpc本身提供了多种服务方法供开发者使用，Faster同样集成并支持这些方法，可通过@GrpcMethod中的type进行修改。

注意：当type做出改变后，对应的参数、返回值也必须相应调整。

接下来的教程将详细讲述这些方法的使用。

更多代码参见：

[faster-grpc-client-example](https://github.com/faster-framework/faster-framework-test/tree/master/faster-framework-test-grpc-client)

[faster-grpc-server-example](https://github.com/faster-framework/faster-framework-test/tree/master/faster-framework-test-grpc-server)

