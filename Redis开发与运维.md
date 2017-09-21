## 一、数据结构
### 1.1字符串
- 基础数据结构
- set key value [ex seconds] [px milliseconds] [nx|xx]
- nx同setnx 键不存在，则设置成功，用于添加，可以作为分布式锁的一种实现方案 http://redis.io/topics/distlock
- xx 键存在，则设置成功，用于更新
- ex同setex 设置键过期时间
- mget key1 key2...批量获取/mset key1 value1 key2 value2...批量设置
- 使用场景
  - 缓存，redis作缓存层，MySQL作存储层，数据先请求redis
  - 设计合理的键名，业务名:对象名:ID:[属性]，例如 vs:user:1或者vs:user:1:name等
  - 计数，视频播放数等
  - 共享session
  - 限速，限制请求频率
```
phoneNum = "138xxxxxxxx";
key = "shortMsg:limit:"+phoneNum;
isExists = =redis.set(key,1,"EX 60","NX");
if(isExists !=null || redis.incr(key) <= 5){
  //通过
}else{
  //限速
}
```

### 1.2 哈希
- 值本身又是一个键值对结构
- 使用场景
  - 相比使用字符串序列化缓存用户信息，哈希类型变得更加直观

### 1.3 列表
- 从左到右组成的有序列表，可以对两端插入（push）和弹出（pop）
- blpop key timeout/brpop key timeout 阻塞弹出
- 使用场景
  - 消息队列，lpush+brpop
  - 文章列表：
```
//每篇文章使用哈希
hmset article:1 title xx timestamp 1234567890 content xxxx
//向用户文章列表添加文章:
lpush user:1:articles article:1
//分页获取文章列表
articles = lrange user:1:articles 0 9
for article in {articles}
  hgetall {article}//可以使用pipeline
```

### 1.4 集合
- 不允许出现重复元素且无序
- sinter/sunion/sdiff key1 key2...交集/并集/差集，对应保存命令：sinterstore/sunionstore/sdiffstore
- 使用场景
  - 用户标签，计算用户共同喜好，推荐系统
  
### 1.5 有序集合
- 不允许出现重复元素，且有序，根据score排序
- zadd key score member [score member]...
- zrank/zrevrank key menber 返回排名 正序（从小到大）/逆序
- 使用场景
  - 排行榜系统
```
//添加赞数
zadd user:rangking:20160316 3 mike
//自增zincrby
zincrby user:ranking:20160316 1 mike
//获取top10
zrevrangebyrank user:rangking:20160316 0 9
```
