# 快速开始

## 引入

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-elasticsearch</artifactId>
</dependency>
```

## 配置

```
spring:
  profiles:
    include:
    - elasticsearch

app:
  elasticsearch:
    host: es地址
    name: 集群名称
```

## 新建实体

```
//type与indexName建议一致，或直接使用_doc（es在新版本中已废弃单index多type的形式）。
@Data
@Document(indexName = "user", type = "user")
public class User {
    @Id
    private String id;
	//可以设置不分词，防止在直接比对中由于标点符号等特殊字符导致无法匹配。
    @Field(type = FieldType.String, index = FieldIndex.not_analyzed)
    private String customerId;
}
```

@Document注解会自动创建索引，在某些场景下可以指定createIndex关闭。

## 新建Repository

```
public interface UserRepository extends ElasticsearchRepository<User, String> {
}

```

Repository提供了基本的增删改查。


## 操作


```
@Service
@Slf4j
@AllArgsConstructor
public class UserService {
    private UserRepository userRepository;
    private ElasticsearchTemplate elasticsearchTemplate;

    public User getById(String userId) {
        return userRepository.findOne(userId);
    }
	public PageResponse page() {
        NativeSearchQueryBuilder nativeSearchQueryBuilder = new NativeSearchQueryBuilder();
        nativeSearchQueryBuilder.withFilter(QueryBuilders.termQuery("id","aaa"));
        nativeSearchQueryBuilder.withPageable(PageRequest.data(1,10));
        Page page = customerRepository.search(nativeSearchQueryBuilder.build());
        return PageResponse.data(page);
    }
}
```

一些复杂的语句可以使用elasticsearchTemplate进行操作。
