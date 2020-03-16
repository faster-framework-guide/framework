# 快速开始

缓存功能开箱即用，但是默认情况下缓存将被存储在项目内存中，而非外置缓存服务器上，如果您需要启用redis缓存，可以参见redis缓存一节。

## 使用
### CacheFacade

我们使用外观模式对缓存进行封装，以达到对外一致。

您可以通过CacheFacade操作管理缓存。

```
   /**
     * 设置缓存
     *
     * @param key 缓存键
     * @param value 缓存value
     * @param exp   失效时间(秒)
     * @param <V>  泛型
     */
    public static <V> void set(String key, V value, long exp) {
        cacheService.set(key, value, exp);
    }

    /**
     * 删除缓存数据
     * @param <V>  泛型
     * @param key 缓存键
     * @return V 泛型
     */
    public static <V> V delete(String key) {
        return (V) cacheService.delete(key);
    }

    /**
     * 清空以cachePrefix开头的缓存
     * @param cachePrefix 缓存前缀
     */
    public static void clear(String cachePrefix) {
        cacheService.clear(cachePrefix);
    }

    /**
     * 获取以cachePrefix开头的缓存数量
     *
     * @param cachePrefix 缓存前缀
     * @return 缓存数量
     */
    public static int size(String cachePrefix) {
        return cacheService.size(cachePrefix);
    }

    /**
     * 获取以cachePrefix开头的缓存键列表
     * @param cachePrefix 缓存前缀
     * @param <K>  泛型
     * @return 返回缓存列表
     */
    public static <K> Set<K> keys(String cachePrefix) {
        return cacheService.keys(cachePrefix);
    }

    /**
     * 获取以cachePrefix开头的缓存值
     * @param cachePrefix 缓存前缀
     * @param <V>  泛型
     * @return 返回缓存列表
     */
    public static <V> Collection<V> values(String cachePrefix) {
        return cacheService.values(cachePrefix);
    }
```

