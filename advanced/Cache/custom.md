# 自定义缓存

如果您不习惯使用redis作为缓存，您也可以集成其他缓存。

## 实现接口

您需要实现ICacheService所有方法，并在Spring中注册您的缓存。

ICacheService代码如下：

```
public interface ICacheService<V> {
    /**
     * 设置缓存
     *
     * @param key 缓存键
     * @param value 缓存值
     * @param exp   失效时间(秒)
     */
    void set(String key, V value, long exp);

    /**
     * 删除缓存数据
     *
     * @param key 缓存键
     * @return V 泛型
     */
    V delete(String key);

    /**
     * 获取缓存数据,如果关键字不存在返回null
     *
     * @param key 缓存键
     * @return 缓存实体
     */
    V get(String key);

    /**
     * 清空以cacheService开头的缓存
     * @param cachePrefix 缓存前缀
     */
    void clear(String cachePrefix);

    /**
     * 查询以cachePrefix开头的cache数量
     * @param cachePrefix 缓存前缀
     * @return 数量
     */
    int size(String cachePrefix);

    /**
     * 查询以cachePrefix开头的keys
     * @param cachePrefix 缓存前缀
     * @return 缓存列表
     */
    Set<String> keys(String cachePrefix);

    /**
     * 查询以cachePrefix开头的值列表
     * @param cachePrefix 缓存前缀
     * @return 缓存列表
     */
    Collection<V> values(String cachePrefix);
}
```


## 注入Spring

```
    /**
     *
     * @return 自定义缓存
     */
    @ConditionalOnMissingBean(ICacheService.class)
    @ConditionalOnProperty(prefix = "app.cache", name = "mode", havingValue = "custom")
    @Bean
    public ICacheService customCacheService() {
        return new CustomCacheService();
    }
```

## 启用配置

在application.yml中

```
app:
    cache:
        mode: custom
```