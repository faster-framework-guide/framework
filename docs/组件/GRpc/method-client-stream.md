#ClientStream
客户端流模式：客户端可发送多条消息给服务端，服务端返回一条消息。

## 服务端

- 服务端定义@GRpcMethod(type = MethodDescriptor.MethodType.CLIENT_STREAMING)
- 客户端的数据通过onNext不断进入，服务端的响应在onCompleted结束后返回。

### 服务实现

注意：
- 返回类型必须为StreamObserver<Integer>，用于获取客户端请求数据，参数必须为StreamObserver<Integer>，用于返回响应数据。

```
@GRpcApi("client-stream")
@Slf4j
public class ClientStreamService {

    @GRpcMethod(type = MethodDescriptor.MethodType.CLIENT_STREAMING)
    public StreamObserver<Integer> testInt(StreamObserver<Integer> response) {
        return new StreamObserver<Integer>() {
            @Override
            public void onNext(Integer value) {
                log.info("请求数据:{}", value);
            }

            @Override
            public void onError(Throwable t) {

            }

            @Override
            public void onCompleted() {
                response.onNext(1);
                response.onCompleted();
            }
        };
    }
}
```

## 客户端

### 接口定义

注意：
- 返回类型必须为StreamObserver，用于传递客户端流给服务器，可以多次使用StreamObserver.onNext()方法传递。
- 参数类型必须为StreamObserver，用于接收服务器端响应。


```
@GRpcService(value = "test-client-stream", scheme = "client-stream")
public interface ClientStreamService {

    @GRpcMethod(type = MethodDescriptor.MethodType.CLIENT_STREAMING)
    StreamObserver<Integer> testInt(StreamObserver<Integer> data);
}
```

### 调用

```
    @Test
    public void testInt() throws InterruptedException {
        StreamObserver<Integer> request = clientStreamService.testInt(new StreamObserver<Integer>() {
            @Override
            public void onNext(Integer value) {
                log.info("返回数据:{}", value);
            }

            @Override
            public void onError(Throwable t) {

            }

            @Override
            public void onCompleted() {

            }
        });
        request.onNext(1);
        request.onNext(2);
        request.onCompleted();
        Thread.sleep(10000);
    }
```

