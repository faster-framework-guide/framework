# 快速开始

## 依赖引入

```
<dependency>
    <groupId>cn.org.faster</groupId>
    <artifactId>spring-boot-starter-mybatis</artifactId>
</dependency>
```

## 配置文件

```
spring:
  profiles:
    include:
    - web
    - mybatis
app:
  datasource:
    host: 数据库地址
    name: 数据库名称
    username: 账号
    password: 密码
```

## 创建实体类

```
@Data
@TableName("tb_user")
public class User {
    private Long id;
    private String name;
    private Integer age;
}
```

## 创建Mapper接口
可以使用mybatis的@Mapper注解，或者直接继承BaseMapper即可，此处演示继承：

```
public interface UserMapper extends BaseMapper<User> {
}
```

BaseMapper实现了诸多常用的方法，可以自行查看，我们无须编写xml文件即可使用内置的方法。


## 创建Service


建立Service，继承ServiceImpl类。

```
@Service
public class UserService extends ServiceImpl<UserMapper, User> {
}
```

ServiceImpl针对BaseMapper进行了优化，如getOne()不会抛出异常，batch时自动分段。
无须注入

## 增删改查

### 查询

```
    /**
     * 根据主键id查询详情
     *
     * @param id 用户id
     * @return 用户详情
     */
    public User queryById(Long id) {
        return super.getById(id);
    }
	 /**
     * 根据条件查询详情
     *
     * @param user 请求参数
     * @return 用户详情
     */
    public User query(User user) {
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        if (!StringUtils.isEmpty(user.getName())) {
            queryWrapper.eq(User::getName, user.getName());
        }
        return super.getOne(queryWrapper);
    }
	 /**
     * 根据条件查询列表
     *
     * @param user 请求参数
     * @return 用户详情
     */
    public List<User> list(User user) {
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        if (!StringUtils.isEmpty(user.getName())) {
            queryWrapper.eq(User::getName, user.getName());
        }
        return super.list(queryWrapper);
    }

```

更多的使用方式，可以直接使用this/super查看。

### 新增

```
    /**
     * 添加用户
     *
     * @param user 实体
     * @return ResponseEntity
     */
    public ResponseEntity add(User user) {
        super.save(user);
        return new ResponseEntity(HttpStatus.CREATED);
    }
```

### 更新

```
	 /**
     * 修改用户
     *
     * @param user 实体
     * @return ResponseEntity
     */
    public ResponseEntity update(User user) {
        super.updateById(user);
        return new ResponseEntity(HttpStatus.CREATED);
    }
```
### 删除

```
	 /**
     * 删除用户
     *
     * @param id 主键id
     * @return ResponseEntity
     */
    public ResponseEntity delete(Long id) {
        super.removeById(id);
        return new ResponseEntity(HttpStatus.NO_CONTENT);
    }
```

## 自定义xml文件

通过以下几个步骤绑定xml与mapper接口。

1. 首先我们需要在包下新建xml文件夹
2. 接下来我们需要建立以Mapper.xml结尾的xml文件，如UserMapper.xml
3. 最后我们按照正常的mybatis xml文件的编写方式编写代码即可。


## 分页
### 基本使用


```
@Data
public class User extends BaseEntity{
	private String name;
}
```

```
    public PageResponse<User> page(User user) {
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        if (!StringUtils.isEmpty(user.getName())) {
            queryWrapper.eq(User::getName, user.getName());
        }
		IPage<User> pageResult = super.baseMapper.selectPage(PageRequest.mybatis(user),queryWrapper);
         return PageResponse.mybatis(pageResult);
    }
```


### 在xml中的分页
对于在Mapper中自定义的方法，我们需要传入Page对象，并且保证在第一位。

BaseMapper:
```
//可以继承或者不继承BaseMapper，不继承时，需要使用@Mapper注解
public interface UserMapper extends BaseMapper<User>{
    /**
     * <p>
     * 查询 : 根据state状态查询用户列表，分页显示
     * 注意!!: 如果入参是有多个,需要加注解指定参数名才能在xml中取值
     * </p>
     *
     * @param page 分页对象,xml中可以从里面进行取值,传递参数 Page 即自动分页,必须放在第一位(你可以继承Page实现自己的分页对象)
     * @param state 状态
     * @return 分页对象
     */
    IPage<User> selectPageVo(IPage page, @Param("state") Integer state);
}
```

xml文件:
```
<select id="selectPageVo" resultType="com.baomidou.cloud.entity.UserVo">
    SELECT id,name FROM user WHERE state=#{state}
</select>
```

service调用：

```

public PageResponse<User> selectUserPage(UserRequest request, Integer state) {
	PageRequest pageRequest = request.mybatis();
	IPage<User> pageResult = userMapper.selectPageVo(pageRequest, state);
    return  PageResponse.mybatis(pageResult);
}
```

### 取消分页

设置pageSize为-1时，即可取消分页。同时不会查询count。

```
PageRequest.mybatis(0,0);//会将0修正为-1。
PageRequest.mybatis(0,-1);
```

### 取消count
有些情况下，分页插件对于复杂的sql无法生成最优的count语句，我们需要编写自己的count。

```
//当 total 为非 0 时(默认为 0),分页插件不会进行 count 查询
pageRequest.setTotal(1);
```

### 获取前N条

如果不使用分页，我们可以使用last为现有sql，增加limit语句，只返回n条数据。

```
 queryWrapper.last("limit 70");
```


