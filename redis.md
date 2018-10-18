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
      save    阻塞  其他命令等待save执行完创建RDB文件，文件策略以老换新  时间复杂度 O（n）

      pgsave  阻塞   客户端执行pgsave 主线程生成子线程进行fork(非常快) 创建RDB文件 以老换新  哦（n）
      自动  根据配置生成RDB文件  是使用pgsave进行生成 
      配置说明
        dbfilename  dump.rdb 默认名字
        stop-write-on-bgsave-error yes  当rdb文件错误不进行保存，默认yes
        rdbcompression yes  是否启用 压缩文件
        rdbchecksum yes  rdb文件的校验
        dir ./  文件目录
       容易忽略的除法方式
       1.全量复制
           没有任何配置以及save 也会成成rdb文件，主从复制
        2.debug reload
        
        3 shutdown 自动生辰文件   


        总结
         1.DDB是redis内存到硬盘的快照拥有持久化
         2.save通常会阻塞redis
         3.bgsave不会阻塞 会产生紫禁城fork
         4,save自动配置满足就会被执行
         5.不容忽视的触发机制
            shutdown 主从复制
13.AOF
   rdb耗时
    复杂度 o(n)
    fork产生子进程消耗内存
    IO性能消耗
   不可控丢失数据
    在写入的时候宕机 丢失数据

  引入AOF
   当执行命令的时候 记录日志AOF

   三种策略
   1.写命令刷新到缓冲区 执行策略。每条命令fsync到硬盘上，生成AOF,ALWAYS
   2.写命令刷新到缓冲区，执行everysec，每秒都刷新到硬盘上，在高性能的时候使用，会产生数据丢失
      宕机谁丢失一秒的数据，有一个默认值

   3.写命令刷新到缓冲区，根据操作系统执行策略，生成aof,
      比较
    always  每条命令都是生成AOP.不丢失数据   但是IO开销大
    everysec  每秒都执行，但是ui丢失一秒的数据 
    no.  不用管  但是不可控
  AOF从写
    就是把一些过期的，复杂的，没有有用的进行优化化简
     减少硬盘占用量
     加速恢复速度
  重写的策略
     客户端 ==》 bgrewriteaof ==>fork子进程==》在内存中重写AOF文件
  自动配置
  auo-aof-rewrite-min-size  重写需要的尺寸
  auto-aof-rewrite-percentage AOF文件增长率

  aof-current-size AOF的当前大小
  aof-base-size AOF上次重启和重写的尺寸

  满足连个条件
  aof-current-size >aoto-aof-rewrite-min-size
  aof-current-size - aud-base-size/aud-base-size > auto-aof-rewrite-percentage
 

  对比
         RDB    AOF
优先级     低     高     两个都重新启动先是加载AOF
体积       小     大     RDB是二进制
速度       块     慢     RDB恢复速度快     
安全行      丢数据  根据策略
轻重      重        轻     rdb中的fork消耗内存  

Redis常见持久化开发运维问题
  1.fork
    同步操作
    与内存息息相关 内存越大 耗时越长 与机器类型有关
    info latest_fork_usec  fork 查看上一次fork消耗的时间
  fork 优化
    优先使用物理机或高效fork的虚拟机
    控制住redis的内存  maxmemory
    合理配置LInux内存你分配策略  
    降低fork频率 例如放宽AOF重写策略，避免不必要的全量复制。会产生bgsave生成RDB

  1CPU
     开销：RDB 和AOF文件，属于CPU密集性 
     优化 不做CPU绑定，不和CPU密集服务部署
         单机不要进行bgsave bgrewriteaof操作

     内存
      开销 fork 内存开销
    硬盘
     开销 ：AOF和RDB文件写入，可以结合iostat .iotop分析       
    优化
      1.不要和高硬盘负载服务部署一起，存储服务，消息队列
      2.no-appendfaync-on-rewrite=yes 在AOF重写时候不追加日志。减少内存消耗
      3.根据写入量决定磁盘类型，例如ssd
      4.单机多实例持久化文件目录可以考虑分盘，限制资源分配
  AOF 追加阻塞
   1. 主线程写入AOF缓冲区 ，同时产生同步线程，刷新硬盘
   2.主线程对比上一次同步时间
   3.大于2秒阻塞，小于2秒通过 ，
   4.。产生问题 
       主进程阻塞，影响执行的发命令
       同步丢失不仅仅丢失一秒数据

  定位
    日志
    info opersistence   每一次阻塞记录次数
redis主从复制
  单机出现的问题
    机器故障
    容量问题
    QBS
  作用
   数据的备份
   读写分离 高可用

   一个mater可以有多个slave
   一个slave只能有一个mater  
    数据流向是单向的， master到slave
   主从复制配置 方式
     1.slavof命令
       取消配置 slaveof no one 这样不会吧数据清除，只不过主节点的数据不会再复制到这个机器
      优先：不用重启 ，但是不好管理
      2。配置  

        slaveof ip port 把这个节点配置ip的从节点
        slave-read-only yes 从节点只能读，不能写防止数据不一致
      优点 好管理  但是要重启
全量复制
1.psync ? -1  从节点不知道runid和偏移量         
2.master 知道是全量复制 把runid和偏移量传给从节点
3.从节点保存主节点信息
4.主节点 bgsave 生成RDB
5.发送send RDB,同时把写命令放到buffer中

6, 发送 buffer
7.清除数据，当从节点加载新的主节点就会清除数据
8.load RDN 同步数据

  全量复制开销
    1.bgsave时间
    2.RDB文件网传输时间
    3.从节点的清除数据
    4.从节点加载RDB时间
    5.可能AOF从写时间
部分复制
1.从节点链接lost 丢失
2.主节点把写命令放到缓存中
3.从节点把自己的偏移量offset和runid 发送给主节点。
   这个offset会和主节点的buffer里面的偏移量对比如果早里面就返回continu
   如果不在说明已经丢失可很多命令
4.发送continue
6同步部分数据

问题
 1.读写分离
    读流量分摊到从节点
 问题
   复制数据延迟
      主节点把数据复制给从节点的时候发生阻塞，导致数据不一制
   读到过期数据， 
      从节点不能删除数据，而同步删除过得数据没有及时同步
      导致。读取到过期时间
   从节点故障
 2.配置不一致
   l例如maxmemory不一致 丢失数据
   例如数据结构和优化参数：内存不一致
 3.规避全量复制
    1.第一次全量复制
       第一次不可避免
         小树节点 底峰
    2.节点云行ID不匹配
    主节点重启
    故障转移
    3.复制缓冲区不足  
      网路终端 部分复制无法满足
      增大复制缓冲配置rel-backlog_size,网络增强
  4，规避复制风暴
  1.单柱节点复制风暴

   问题 主节点重启，多从节点复制
   解决 跟换复制拓扑
 2 单节点复制风暴
   问题：机器宕机后，大量的全量复制
   解决：直接分散多机器

第八章 redids sentinel

    1.主从复制高可用
     问题：手动故障转移
        1.主节点有问题
        2.slave 建立新的主节点
        3.其他从节点重新复制新的主节点


     问题 ： 写能力和存储能力受限   

  Redis Sentinel故障转移

1.多个sentinel发现master有故障
2.选举一个sentinel作为领导
3.sentinel 选出一个slave作为master
4.其他的slave成为新的master的slave
5.sentinel通知客户端新的master
6.等待老的master复活成为新的master的slavae

sentinel主要配置
   port 端口
   dir 目录
   logfile 日志文件
   sentinel  monitor mymaster 127.0.0.1 2 
      监控master mymaster 名称  IP  2代表几个sentinel认为主节点出现故障就认为有问题
  sentinel down-after-milliseconds mymaser 3000
     代表sentinel多少秒一直Ping不通就代表有问题  
  sentinel parallel-sync mymaster 1    每次异步复制一个
    sentinel failover-timeout mymaster 18000 
      转移时间 

java客户端
 1.遍历所有的sentinel集合获取一个可用sentinel 参数sentinel结合 he
 和mastername
 2. 通过sentinel 根据mastername获取master信息，返回给sentinel
 3.当客户端获取master信息的时候，会做一次role 角色的验证
 4.sentinel发现master节点变化发布消息给客户端（内部实现发布订阅）

坑记录
  问题：All sentinels down, cannot determine where is mymaster master is running.

这是因为哨兵的配置文件里的保护模式启动了。所以又因为没有绑定可以访问的ip和设置访问密码，就不允许从外部访问。

我们的哨兵明明是127的地址，应该访问127.0.0.1:26379才对啊，这是因为redis内部把127的地址转换成了192.168.2.101了，也就是你的本机的ip地址。所以访问192的地址就相当于从外部访问你的哨兵。

根据它的1，2，3，4的提示，你只要满足其中一个就行。于是我就把三个哨兵的sentinel.conf配置文件中的保护模式改为禁用就行了


三个定时任务
 1.每10秒每个sentinel对master和slave进行info
   发现slave节点
   确定主从关系
 2.每2秒每个sentinel 通过master节点的chananel交换信息
      通过_sentinel_:hello频道交互
      交互对节点的“看法”和自身信息
 3.每一秒每个sentinel对其他sentinel和redis执行ping
   心跳检测，失败判定依据

主观下线和客观下线
  sentinel monitor mymaster ip port quorum
  sentinel monitor down-after-milliseconds mymaster 30000
 主观下线 ，每一个sentinel节点对Redis节点失联，判定下线
 客观下线 。多有sentinel节点对redis节点失联，达成共识，超过quorum数目

 sentinel is-master-doen-by-addr 判断主节点是否下线 和是否成为一个领导职
  sentinel节点最好为奇数而quorum最好是(总数/2+1)


redis领导选举
原因只有一个sentinel节点完成故障转移
选举 通过sentinel is-master-down-by-addr命令。希望成为领导者

1.每个主观下线的sentinel节点想起他sentinel及节点发送命令，请求自己为领导者
2.当其他sentinel收到命令时候，其他sentinel发现自己还没有同意其他节点通过，这就通过，否者拒绝
3.如果该sentinel发现自己的票数超过qurum.就成为领导者
4.此时也会出现多个sentinel节点成为领导者，则等待一段时间继续选举


故障转移
 1从slave节点先出一个合适的节点作为主节点
 2对上面的slave节点使用slavae no one 成为主节点
 3.向其他节点发送命令让他进行成为新的master的从节点 。复制参数和parallel-syncs参数有关
 4.更新对原来master节点配置为slave.并记性关注，当复活的时候进行新节点master节点复制

  合适的节点为master
  1.选择优先级搞得 slavae-priority,如果存在返回否则继续
  2.选择偏移量最大的（和master比较接近，数据差距小），如果有则返回，否则继续
  3.选在runID细小的salve节点

 节点运维
  1机器下线
  2.机器性能不足
  3.节点自身故障

  主节点
   sentinel failover msatername
     只有故障转移。他已经确认master下线以及领导是谁
   三个消息
   1.switch-master切换主节点
   2convert-to-slave切换到从节点
   3.sdown -主观下线  
redis 集群
 顺序分区 和哈希分区 


 哈希分区        数据分散度高           memeCACHE
                键值分布业务无关
                无法顺序访问
                支持批量操作

 顺序分区       数据分散易倾斜
               键值业务相关
               可顺序访问
               不支持批量                BigTable  HBase


节点取余分区
  hash(key)%3
  添加一个节点
    迁移80%数据 ，可以多陪扩容约为50%
  问题：数据节点变化，导致数据迁移
  迁移数量和添加节点数量有关：建议多陪扩容

一致性哈希
 客户端分片 哈希+顺时针（优化取余）
 节点伸缩：只影响领巾节点但是还是有数据迁移
 翻倍伸缩：保证最小迁移数据和负载均衡

 虚拟槽分配

  每一个槽都有一个范围
  每一个槽对应一个节点
  每一个可以进行哈希计算如CRC16(KEY)&16383
  计算的结果放到node1如果是就放到这里保存
  如果不是就返回一个node,进行保存
redis cluster
  节点  设置  redis-cluster-enable yes 
  meeet  节点之间相互通信
  指定槽   默认指定槽的范围16384 
特点
  复制 主从复制
  高可用 主节点有问题，从节点顶上
  分片 多个主节点进行读写


原生命令安装
   1.节点配置
      cluster-enable yes
      cluster-config-file nodes-port.conf

   2.meet
       cluster meet ip port
   redis-cli -h 127.0.0.1 -p 7000 cluster meet 127.0.0.1  7001


  主要配置
    cluster-enabled  yes
    cluster-node-timeout 15000
    cluster-config-file “nide-conf”
    cluster-require-full-coverage yes  表示一个节点有问题集群就不提供货服务
  3.分配槽
   cluster addslots  {0-5461} 
  4.设置主从
     cluster replicate  node-id  (node 节点不变)
  redis-cli -h 127.0.0.1 -p 7003 cluster replicate ${node-id}   

1.原生命令安装
 繁琐 ，生产环境不使用
2.官方的工具
  高效  准确 ，生产环境您可以使用
3.其他
  可视化部署

  第十章 集群伸缩

  1、伸缩原理

    集群伸缩=槽和数据在节点之间的移动

  2.扩容集群
    准备新节点
    加入集群
    迁移槽和数据 

  新节点
    集群模式
    配置和其他节点统一
    启动后是孤儿节点
  加入 集群
   cluster meet 127.0.0.1 6385   
    作用
      为它迁移槽和数据实现扩容
      作为从节点负责故障转移


迁移数据

 1.对目标节点发送 cluster setslot {slot} importing {sourceNodeid} ,让目标节点准备导入槽的数据
 2.对源节点发送 cluster setslot {slot} migrating {targetNodeId}命令 让源节点准备前除草的数据
 3.源节点循环执行clustergetkeysinslot {slot} {count}命令 ，每次获取count个属于槽的键
 4.在与阿尼额点执行 migrate {targetIp}{targetPort} key 0 {timeout} 命令吧指定key迁移
 5.重复执行不走3-4h直到槽下所有的键数据迁移目标节点
 6，想集群内所有的节点发送clustersetslot {slot} node {tartgetNodeId}命令 通知槽分配给目标节点


 集群扩容
  1.新建节点
  2.加入集群
  3.设置主从
  redis-cli -p 7007 cluster replicate 73c53ccef2e444fe3a65cf582398af07a784302c
  4.迁移槽
  redis-trib.rb reshard 127.0.0.1:7000

 收缩集群 
   
  1.下线迁移槽
  2.忘记节点
     cluster forget {downNodeId}  忘记 nodeid
  3.关闭节点

      使用 redis-trib.rb del-node 127.0.0.1:7000 73c53ccef2e444fe3a65cf582398af07a784302c

客户端路由

moved重定向

 1.客户端执行命令想一个节点
 2，如果节点命中，执行命令
 3.否则，返回异常moved 带有节点信息
 4.客户端在执行命令

 cluster keyslot hello 计算key的slot


 ask重定向


 1.当客户端去获取值时候，获取源节点
 2.但是源节点的数据迁移到了目标节点
 3.这是就应该有一种机会 ask重定向


 两个重顶向
   都是是客户单重定向

   moved草已经确定迁移
   ask，槽还在迁移中

问题
  性能

 smart 客户端
  JedisCluster  


 批量优化
 
  1.串行mget
    循环遍历在对应的节点获取可以，
    n次网络时间
  2.串行IO  
    把客户端的key算出对应的槽，本地槽对应了节点
    然后不同的槽建立一个子集合，执行pipline ,jian\
    减少网络消耗
  3.并行IO
    和串行前面一样，后面启动多个线程并行执行，并行执行一次
    pipline  
  4.hash_tag
    使用tag吧可以保证起来， mget冲一个节点获取


故障转移


主观下线
 定义：某个节点认为另一个节点不可用
两个节点 ，节点1向节点2ping
 如果通了就返回pong,然后同步最后一次通讯时间
 如果不同，再次执行ping,超过时间就认定不可用
 
 客观显现
 当半数以上持有槽的主节点都标记某节点下线


 故障恢复

  1.资格检查
   每个从节点检查与故障主节点的短线时间
   超过cluster-node-timeout*cluster-slcae-validity-factor
   cluster-slave-validity-factor 默认10
    也就是150毫秒
    就是认为时间太长
  2.准备选举时间
    复合资格，就会减少延迟选举时间，一遍尽快进行选举，获取更多的票数，实际就是诶可护具的一致性
  3.选举投票
     大于主节点一般，就可以替换

  4替换
    当前从节点取消复制变为主节点
    执行clusterdelslot撤销故障主节点负责的槽，并执行
    clusteraddslot把这些草分配给自己
    想集群官博字节的pong消息，表明已经替换了故障丛节点   

集群完整性

 cluster-require-full-coverage 默认yes
  集群中16384槽全部可用，保证集群完整

  节点故障或者正在故障转移
  报错
  大多业务无法容忍 设置为no

  贷款消耗
  1.消息发送频率，节点和其他及诶按超过cluster-node-timeout/2
  就会发送ping
  2.消息数据量：slots槽数组（2kb）和整个集群的状态数据
  3.节点部署规模

  优化
   避免大集群：避免多业务使用一个集群
   cluster-node-timeou：带宽和故障转移时间
   计量均匀分配到多台机器上：保证高可用和宽带


3，pub/sub广播
  publish在集群每个节点广播：加重带宽

  单独走一台RedisSentinel

 4.集群倾斜
      数据倾斜：内存不均
     请求倾斜 ：热点 
   原因
     节点和槽分配不均
     redis-trib.rb info IP：port  查看节点 槽

     redis-teib.rb. rebalanxe 均衡槽 和节点
     不通槽对应键值数量差异较大
       存在hsah_taghsah_tag
       cluster countkeysinslot {slot}获取槽的keyd
       的数量

     包含bigkey
      大字符串 几百万的元素
      redis-cli bigkeys
      优化数据结构
     内存相关配置不一致
      定期查查配置一致性



 数据倾斜 -- 请求倾斜

  热点key 重要的key或者bigkey
   优化

    避免bigkey
     热键不要用hsah_tag
     当一致性不高时候，可以使用本地缓存


  读写分离
  
   只读链接：集群模式的从节点不接受任何读写请求
   从定向发哦负责槽的主节点
   readonly命令可以读，链接级别命令

  读写分离

   不建议在集群中使用
    复制延迟， 服务过期时间 从节点故障

    客户端维护槽和节点的关系 cluster slave {slots}   
       
6.数据的迁移


官方迁移redis-trib.rb IMport
 只能充单机迁移到集群
 不支持在线迁移
 不支持断点续传
 单线程迁移：影响速度

  在线迁移

   唯品会reids-migrae-tool
   豌豆荚redis-port

集群和单机对比

 key 批量操作支持优先。例如mget.mset必须在一个slot

 key事物和Lua支持有限：操作的key必须在一个节点（hash_tag）

 key数据分区的对小力度：不支持bigkey分区


不支持多个数据库，一个对比0
复制只支持一层，不支持属性复制结构



总结

 rediscluster 数据分区规则采用虚拟槽方式，每个节点负责一部分槽和相关数据，实现数据和请求的负载均衡

 搭建环境划分4个步骤，准备节点，节点握手，分配槽，复制 可以使用，redis-trib.rb

 集群扩容
  节点之间移动槽和相关数据
  扩容：根据曹迁移计划吧曹冲源节点迁移到新节点
  收缩:下线节点负责的槽需要迁移到其他节点，在使用cluster forget忘记节点，
  多有集群节点忘记下线节点

  使用smart客户端操作达到通信效率最大化，客户端内部负责计算维护键-》槽-》节点的映射
  用于快速定位到目标节点

  集群自动故障转移过程分为故障发现，和节点回复，
  问题超发规模集群款带消耗 广播 ，数据倾斜，单机集群对比


redis缓存

 1，收益
    加速读写
    降低后端负载
 2.成本
    数据不一致
    代码维护成本 
    运维成本 Rediscluster
使用场景
 降低后端负载
  对高消耗的sql 
 加速响应时间
 大量协和并为批量写 

 redis更新策略     
                    一致性     成本
  LRU/LIRS算法踢出     最差     最低

  超时踢出           较差     中

  主动更新          强        搞

  建议
  1，低一致性  最大内存和淘汰策略
  2.高一致性 ：超时剔除个主动更新结合 最大内训和淘汰

 缓存力度控制

  1.通用性：全量属性
  2，占用空间 部分属性
  3，代码维护，表上全量属性更好

  缓存穿透问题

  1缓存不命中
    获取缓存一个空值values
     可能是代码原因，设置一个空值
     恶意攻击，传了一个没有用的值
  发现问题
  2.业务的响应时间
     一般是10秒，出现100秒可能就要问题
   3.业务本身问题



   缓存空对象
    1.可以设置一个过期时间
    2.

无底洞问题优化

 增加机器不一定会性能优化可能会增加网络请求

  优化

    慢查询命令  mget mset
    减少网络请求
    链接池，长链接
  方案分析
    串行mget n此网络请求
    串行IO 节点此网络请求
    并行IO  减少时间
    hash_tag  维护成本大 ，


 热点key重建过程
   会导致大量的查询数据源和重建缓存，影响响应时间和性能
解决
  1.减少缓存重建次数
  2.数据尽量一直
  3减少前端风险

  互斥锁
     当进行获取缓存是后，加锁，让其他线程不进行重建缓存
     知道解锁，然后输出结果
   

  永远不过期，但是设置values业务过期时间
    线程1无等待获取缓存
    线程2一样
    线程3发现缓存过期，构建缓存
    线程4重建完成，获取新的输出

    导致数据不一致























  











    



















 





















