# BidiStream
双向流模式：客户端传递多条数据，服务器端响应多条数据。

## 服务端

- 服务端定义@GRpcMethod(type = MethodDescriptor.MethodType.BIDI_STREAMING)
- 客户端的数据通过onNext不断进入，服务端可以不断调用onNext写回。

### 服务实现

注意：
- 返回类型必须为StreamObserver，用于接收客户端参数。
- 参数类别必须为StreamObserver，用于返回服务端响应。

```
@GRpcApi("bidi-stream")
@Slf4j
public class BidiStreamService {

    @GRpcMethod(type = MethodDescriptor.MethodType.BIDI_STREAMING)
    public StreamObserver<Integer> testInt(StreamObserver<Integer> response) {
        return new StreamObserver<Integer>() {
            @Override
            public void onNext(Integer value) {
                log.info("请求数据:{}", value);
                response.onNext(1);
            }

            @Override
            public void onError(Throwable t) {

            }

            @Override
            public void onCompleted() {
                response.onCompleted();
            }
        };
    }
}
```

## 客户端

### 接口定义

注意：
- 返回类型必须为StreamObserver，用于不断发送消息给服务端。
- 请求类型必须为StreamObserver，可以不断接收消息。

```
@GRpcService(value = "test-bidi-stream", scheme = "bidi-stream")
public interface BidiStreamService {
    @GRpcMethod(type = MethodDescriptor.MethodType.BIDI_STREAMING)
    StreamObserver<Integer> testInt(StreamObserver<Integer> data);
}
```

### 调用

```
    @Test
    public void testInt() throws InterruptedException {
        StreamObserver<Integer> request = bidiStreamService.testInt(new StreamObserver<Integer>() {
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
        Thread.sleep(1000);
    }
```