                          第一章 Redis初始
redis 是什么
 1.开源
 2.键值对的储存服务（数据库）
 3.多种数据节后
  字符串 hashtable linklist（列表） set（集合）  sortset(有序集合)
 4.功能丰富 
 Redis特点
  1.速度快
   数据存储在内存
   C语言编写
   单线程
   主要是因为使用内存，寄存器>一级缓存》二级缓存》内存》本地硬盘》远程硬盘
  2.持久化
  redis会把数据存储到内存，数据更新会异步同步到硬盘
  3，数据结构
   字符串 hashtable 链表 集合 有序集合 位图 GEO定位地理位置 小的内存记录的唯一值 
  4.多种语言
  5.功能丰富
   消息发布订阅  Lua脚本  事物  pipeline提高并发效率
  6.简单
   代码少 单线程
  7.主从复制
  8.高可用分布式
    高可用 2.8 版本redis sentinel 支持高可用
    分布式 3.0 Redis cluster支持分布式
 redis使用场景
  缓存  计数器 消息订阅  实时系统 社交网络 

 redis 安装使用
  启动
   1.直接使用redis-server
   2使用动态参数
   3.配置文件使用
 redis 常用配置
   daemonize 是否是守护进程 默认no
   port
   logfile redis系统日志
   dir  redis工作目录 
 4.redis api 使用和理解
  4-1通用命令
    keys 获取key值
    一般不建议的使用因为比较中，redis是单线程， io阻塞
    dbsize key的总数
     是计数器记录总数
     exist 检查key是否存在
     del 删除key-value
     expire key second 给key设置过期设置
     ttl key  查看过期剩余时间 -2 表示key不存在 -1 key存在，没有过期时间
     persist key 
     去掉key的过期时间 
     type key 查询可以的类型
      返回 string list set  zset hash none(表示不再key)
      sadd 添加集合

5.单线程
  就想一条高速公路，每一次都会自行一个命令其他命令在等待
  5-1.为什么这么快
   纯内存
   非阻塞IO
   避免资源切换，资源竞争
  5-3.注意
   一次只执行一个命令
   拒绝长命令
   keys flushall flushdb 
   其实不是单线程
      fysnc file descriptor
      close file descriptor
6.字符串
  内部可以是字符串，也可以是整数 也可以是bits，而二进制 可以是 json xml 
  values 不能大于512MB 建议最多100mb 太大影响速度，有网络开销 
  场景
    缓存 计数器  

  get set  del 
 
  incr key 
   key 自增1 如果key不存在，自增get(key)=1
  decr key 
 key 自减1 ，不存在 =-1
 incrby key k
 key 自曾 k 不存在 =k

 decrby key k
 key 自减 k  不存在 =-k

 可以使用自增id


 set key不管存不存在都设置
 setnx  key不存在才设置
 set xx  keyc存在 设置
 mget 批量获取  0（n）
 mset 批量设置  o(n)
 getset  设置新的值获取就得值
 append key value 追加值
 strlen key 字符串长度
 increbyfloat 增加key的值浮点
 getrange key start end 获取指定位置的值
 setrange key index value 设置指定位置key的值

 7.hash 哈希

   hget key file values 
   特点
    mapmap
    小的Redis
    file 不能相同


    hget 获取file的值
    hset 设置file的值
    hdel 删除file的值
    hexist 是否fied 
    hlen 获取 hash key field 的数量
    hmget key file1 file2 批量获取filed得值  0(n)
    hmset key files valus1 file2 values2  O(n)
    hgetall key 获取多有file和value（小心使用）
    hvals key 获取所有fies的值
    hkeys key 获取所有files的值
   hsetnx key fiels value 设置对应fied的value
   hincrby key field intCoutn 设置key 对应field的values的自增
   hincrbyfloat 设置对应key 的files 的vakuse自增

8.list
  特点  有序 可以重复 左右两边插入弹出
   增
   rpush 从右边新增
   lpush 从左边新增

   linsert key before|after value newValue 插入指定位置前后newvalue
   
   删
    rpop 从右边删除一个
    lpop 从左边删除一个

    lrem key count value 
     根据count值，从列表删除value 最多删除count个值，相等的value
     count >0 从左到右  做多删除count值
     count<0  删除所有的value 相等的值

   改
    ltrim key start end 按照索引范围修建列表
     ltrim mylist 0 1
     lset key index value
   查
   lrange key  start end  根据下表获取列表
   lindex key index 获取指定索引的item

   llen 列表长度
   blpop

9 set
   特定 ：无序  无重复 集合操作
  sadd key 设置
  srem  删除

  sinter 集合交集
  sdiff 集合差
  sunion 集合合集

  scard 计算集合数量
  sismember 是否存在元素
  smembers 获取所有元素  无序 小心使用
  srandmember 随机获取元素
  spop 从集合中弹出元素随机

  10有序集合
     特点 ： 
      结合是有序 ，
      zadd 添加有序集合
      zrem key element删除
      zscore 获取分值
      zrank 获取等级
      zrange 获取范围数值  withscores 携带分数 低到高排序
      zrangebyscore zmyset 100 300 withscores根据分数获取
      zcount  key min max 最大分值到最小分值的数量

      zremrangebyrank  删除等级范围内的值
      zremrangebyscore 删除分组范围内的值
      zincrby zmyset 10 s 增加集合元素的分值

11.jedis客户端
    
  直连
    优点 简单方便 适用于少量长期链接场景
    缺点 存在每次新建关闭TCP开销
         资源无法控制，存在链接泄漏的可能
  连接池
     优点 jedis预先生成，降低开销
     连接池的形式保护和控制资源的使用
     缺点 相对直连麻烦，尤其在资源的管理上需要很多参数
     保证，一旦规划不合理也会出现问题


 瑞士军刀Redis
   1.慢查询
     生命周期
      clinet-->1.发送命令---》2.排队--》3.执行命令--》4.返回结果
       说明 慢查询发生在执行命令 ，客户端超时不一定是慢查询，但慢查询是客户端超时的一个可 能原因     
   2两个配置 
     2-1 slowlog-max-len 
        先进先出队列
        固定长度
        保存在内存中
     2-2 sloelog-log-slower-than
      慢查询阀值
      slowlog-log-slower-than=0记录全部命令
      slowlog-log-slower-than<0,不记录任何命令   
  3.配置方法
   默认值
     config get slowlog-max-len=128
     config get slowlog-log-slower-than=10000 微妙
   修改配置重启
   动态修改配置
   config set slowlog-max-len 1000
   config set slowlog-log-slower-than 1000

   4.慢查询命令
     slowlog get [n] 获取慢查询队列
     slowlog len 获取慢查询队列长度
     slowlog reset 清空慢查询队列
   5.经验
    slowlog-max-len 队列长度 默认 128 建议设置1000，过少会导致丢失
    slowlog-log-slowler-than 阀值 默认10ms ,建议1ms
    理解生命周期
    定期持久化慢查询

   6.pipeline
     什么事流水线
      将多次命令打包 服务端进行计算  根据顺序返回结果

      redis的毫秒级别
      pipeline每次条数要控制（网络）主要是控制网络时间
      pipelin在java中实现
       依赖 redis.clients
           jedis
       原生的是原子的
       pipeline非原子

       建议
         1.每次注意pipeline携带数据量  拆分
         2.pipeine每次只能作用在一个节点
         3.和原生的区别

    7.发布订阅
       7-1角色
        发布者 订阅者  频道
       7-2模型
       7-3API
         publish
           
           publish channel message 发布消息

         subscribe

           subscribe channel 订阅

         unsubsrcbe
           unsubscribe channel 取消订阅
         其他
       psubsrcibe pettern 订阅模式
       punsubsrcibe pattern 退订指定模式
       pubsub channel 列出至少有一个订阅的频道
       pubsub numsub channel  列出给定频道的订阅者数量
       pubsub numpat 列出被订阅模式的数量


       消息队列
          消息是抢的功能 只能有一个收到消息 使用list实现，阻塞

        发布订阅的角色
        消息队列 和 发布订阅
        API
   8.bitmap
   
    setbit
    getbit
    bitcount 获取位图指定范围
    bitop op deskey key  做多个Bitmap 的(and)交集 (or)并集  (not)非  (xor)异或  
    操作并把结果发到deskey中

    bitpos key targetBit [start][end] 不指定就获取全部范围
      获取指定范围的第一个偏移量对应值等于targetbit位置 

  9.HyperLogLog  
   
     算法：极小空间完成独立数量统计 
     还是一个字符串

     pfadd key element  想hyperloglog添加元素

     pfcount key 计算hyperloglog独立总数
     pfmerge deskey sourcekey 合并多个hyperloglog

     是否能容忍错误 0.81%
     不能去除单条数据
   10 GEO
     GEO 是啥
      地理信息定位 存储经纬度 计算两地距离 范围计算等 
      如北京89公里范围内的饭店 

       如微信的摇一摇  周围的九点

       geoadd 添加地址位置信息
       geopos 获取地理位置信息
       geolist 两个地址的距离  单位 m km mi  ft 
       georadius   获取指定位置范围地理位置信息集合

       3.2提供功能
         type  zset
          可以使用zrem  key member

11.redis持久化

    持久化作用
      redis把数据存储在内存上，数据更新异步的同步到磁盘上

     持久化方式
     
       1.快照    mysql Dump  redis RDB
       2.日志    mysql binlog  redis AOF   Hbase Hlog  

12 RDB
      什么是RDB
      redis把内存的数据生成一个RDB文件（二进制）
      触发的三种方式
      save    阻塞  其他命令等待save执行完，文件策略以老换新  时间复杂度 O（n）
       
      pgsave  阻塞  
      自动 

    



















 





















