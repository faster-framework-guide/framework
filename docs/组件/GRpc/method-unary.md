# Unary
一对一模式：一条客户端请求对应一条服务端响应。Java中又支持使用ListenableFuture与StreamObserver异步获取服务端消息。

## 基本

### 服务端

#### 服务实现

注意：
- 此处的参数必须一致：第一个为业务参数，第二个为响应参数。

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

### 客户端

#### 接口定义

```
@GRpcService(value = "test-unary", scheme = "unary")
public interface UnaryService {
    @GRpcMethod
    int testInt(int data);
}
```

#### 调用

```
@Test
public void testInt() {
    int result = unaryService.testInt(1);
    log.info("返回数据:{}", result);
}
```


## ListenableFuture

### 服务端
无须做修改。

### 客户端

接口定义：

注意：
- 返回类型必须为ListenableFuture

```
@GRpcService(value = "test-unary", scheme = "unary")
public interface UnaryService {
    @GRpcMethod("testInt")
    ListenableFuture<Integer> testListenableFuture(int data);
}
```

调用：

```
    @Test
    public void testListenableFuture() throws ExecutionException, InterruptedException {
        ListenableFuture<Integer> listenableFuture = unaryService.testListenableFuture(1);
        int result = listenableFuture.get();
        log.info("返回数据:{}", result);
    }
```


## StreamObserver

### 服务端
无须修改

### 客户端

#### 接口定义

注意：
- 返回值类型必须为void
- 参数必须存在两个：第一个为业务参数，第二个为响应参数，顺序必须一致。

```
@GRpcMethod("testInt")
void testStreamObserver(int data, StreamObserver<Integer> streamObserver);
```

#### 调用
由于是异步、test方式，此处的Thread.sleep(10000)为了看到结果，否则线程结束。

```
    @Test
    public void testStreamObserver() throws InterruptedException {
        unaryService.testStreamObserver(1, new StreamObserver<Integer>() {
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
        Thread.sleep(10000);
    }
```

