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
 
 setnx
 set xx








 





















