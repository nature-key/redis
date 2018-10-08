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

 





















