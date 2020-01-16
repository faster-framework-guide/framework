# ServerStream
服务端流模式：客户端请求一条数据，服务端返回多条数据。

## 服务器
- 服务端定义@GRpcMethod(type = MethodDescriptor.MethodType.SERVER_STREAMING)
- 参数StreamObserver<Integer> response为服务端响应数据，可以多次调用response.onNext()返回数据。

### 服务实现

注意：
- 方法第一个参数为业务实体，第二个为响应流。顺序必须一致。

```
@GRpcApi("server-stream")
@Slf4j
public class ServerStreamService {

    @GRpcMethod(type = MethodDescriptor.MethodType.SERVER_STREAMING)
    public void testInt(int data, StreamObserver<Integer> response) {
        log.info("请求数据:{}", data);
        response.onNext(1);
        response.onNext(2);
        response.onCompleted();
    }
}
```

## 客户端
### 接口定义


注意：
- 返回值类型必须为Iterator，方法参数为业务实体。

```
@GRpcService(value = "test-server-stream", scheme = "server-stream")
public interface ServerStreamService {
    @GRpcMethod(type = MethodDescriptor.MethodType.SERVER_STREAMING)
    Iterator<Integer> testInt(int data);
}
```

### 调用

```
    @Test
    public void testInt() {
        Iterator<Integer> result = serverStream.testInt(1);
        result.forEachRemaining(item -> {
            log.info("返回数据:{}", item);
        });
    }
```