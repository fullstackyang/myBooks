## 数据结构
### 字符串
- 基础数据结构
- set key value [ex seconds] [px milliseconds] [nx|xx]
- nx同setnx 键不存在，则设置成功，用于添加，可以作为分布式锁的一种实现方案 http://redis.io/topics/distlock
- xx 键存在，则设置成功，用于更新
- ex同setex 设置键过期时间
- mget key1 key2...批量获取/mset key1 value1 key2 value2...批量设置
#### 典型场景
