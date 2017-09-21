## 一、数据结构
### 1.字符串
- 基础数据结构
- set key value [ex seconds] [px milliseconds] [nx|xx]
- nx同setnx 键不存在，则设置成功，用于添加，可以作为分布式锁的一种实现方案 http://redis.io/topics/distlock
- xx 键存在，则设置成功，用于更新
- ex同setex 设置键过期时间
- mget key1 key2...批量获取/mset key1 value1 key2 value2...批量设置
#### 1.1 典型场景
- 缓存，redis作缓存层，MySQL作存储层，数据先请求redis
- 设计合理的键名，业务名:对象名:ID:[属性]
