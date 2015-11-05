title: Redis 学习笔记
date: 2015-11-03 15:30:45
tags: Redis
categories: 学习笔记
feature: img/redis-300dpi.png
toc: true
recommend: true
declaration: true
comments: true
---


前段时间项目中用到Redis，是我第一次真正的使用Redis。最近工作不是很忙就根据官网的入门教程进行下简单的学习，并做了简单的笔记（就是简单的翻译一下）。还没有全部学完以后有时间会慢慢补充。

<!--more-->

## Redis 数据类型和抽象

***

### 简介

   redis支持一下几种数据类型

1.  二进制安全字符串（Binary-safe strings）

2.  列表（Lists）   
    基于linkedlist，以插入顺序排序，元素为字符串类型

3.  无序集合（Sets）    
    元素为字符串，无序且唯一

4.  有序集合（Sorted sets）    
    类似于无序集合，但是有序集合中的每个string元素会关联一个成为score的float值，元素总是根据他们的score进行排序

5.  哈希（Hashes）   
    即由一个filed关联一个value的map，field和value均为string

6.  简单的位图（Bit arrays or simply bitmaps）    
    ***目前还不懂是什么类型**

7.  HyperLogLogs    
    ***这个也是不懂**

### Redis的key介绍
redis 可以使用任何二进制序列作为key的值，一个空的字符串也可以 
下面是一些key值的使用规则

1.  key的值不应太长，太长的话会影响浪费存储空间，并且影响查询效率
2.  key的值也不应该太短，应该尽量能表达出key要表达的意义；所以key的值应该考虑占用空间尽量小，且能明确表达出其意义
3.  尽量使用一个固定模式，例如object-type:id
4.  key值允许最大值为512M

**注意：** Redis的执行写操作时，如果key不存在，Redis将自动创建key；执行读操作时，对于聚合类数据类型，如果数据元素为空时将销毁对应的key  
针对不同的数据类型，redis有不同的命令，不可混合

### String 类型

string 类型是用于redis key的最简单的数据类型，也是key经常使用的数据类型


### List 类型

Redis中的List类型通过**linkedlist**数据结构实现，所以插入的效率很快，但是通过*index*获取数据会很慢

List类型常用于以下两种情况

*   社交网络中记录用户最后一次的更新
*   进程间通信，使用生产者消费者模式；针对这种使用形式，Redis提供了特殊的命令来保证高可靠性和高效率

### Hash 类型

Redis中的Hash类型类似于java中的Map类型，是一个field-value键值对

### Set 类型


***
***

## Redis 常用命令

***

###  String 常用命令

1.  `set` 命令
    
    如果key应经存在，则进行覆盖，key和value的大小不可超过512M
    
    *   使用格式
            set key value [options]
		常用选项：		
		|选项|说明|
		|------|--------------------|
		| `nx` | key存在时执行失败  |
		| `xx` | key不存在时执行失败|
    *   ex
            
            set mykey myvalue

1.  `get` 命令
    
    根据key获取值

    *   使用格式
            
            get key 

    *   ex
            
            get mykey

1.  `incr` 命令
    
    自增长命令,可以将一个字符串解析为一个整型，默认步长为1，将得到的新值存储到key下，该操作是原子性的

    其他类似的命令`incrby`、`decr`、`decyby` 

    *   使用格式
        
            incr key [step]

    *   ex
        
            set counter 100
            incr counter
            incr counter 50

1.  `getset`
    
    返回原key的值，并设置新的值
    *   使用格式
        
            getset key newvalue

    *   ex 
        
            getset mykey mynewvalue

1. `mset`，`mget`
    
    多个key的设置与值获取
    使用mget时会返回一个数组

    *   使用格式
        
            mset key1 value key2 value2 key3 value3 ...

            mget key1 key2 key3 ...

    *   ex
            
            mset a 10 b 20 c 30

            mget a b c

1.  [String 更多命令](http://redis.io/commands#string "More Strings operators")

### key 常用命令
    
1.  `exists`
    
    判断给定的key是否存在，存在返回1，否则返回0

    *   使用格式
        
            exists key

    *   ex
    
            exists mykey

1.  `del`

    删除指定的key，key存在返回1，否则返回0

    *   使用格式
        
            del key

    *   ex
        
            del mykey

1.  `type`
    
    查看key的数据类型

    *   使用格式
            
            type key

    *   ex
        
            type mykey


1.  `expire`

    设置key的存活周期，时间精度为秒；存活周期信息会被复制并持久化到硬盘

    *   使用格式
        
        对已经存在的key进行设置

            expire key times

        在初始化时设置

            set key value ex times

    *   ex
        
            expire mykey 5

            set mykey 100 ex 10

1.  `ttl`
    
    查看key存活期生意时间

    *   使用格式
        
            ttl mkey

    *   ex
            
            set mykey 100 ex 10

            ttl mykey

1.  `pexpire`，`pttl`
    
    设置和查看存活周期剩余时间，功能同`expire`和`ttl`

1.  [Key 的更多命令](http://redis.io/commands#generic "More keys operators")

### List 常用命令

1.  `lpush`，'rpush'
    
    向list中添加数据，`lpush`在左边（头部）添加数据；`rpush`在右边（尾部）添加数据

    命令将返回添加数据后的list的大小

    *   使用格式
        
            rpush list value1 value2 value3 ...

            lpush list value1 value2 value3 ...

    *   ex

            rpush mylist A B C

1.  `lrange`

    提取给定范围（两个index）的数据；index可以为负数，负数表示尾部数据，例如 `-1` 表示最后一条数据、`-2` 表示倒数第二条数据

    *   使用格式

            lrange list index1 index2

    *   ex

            #返回第一条到最后一条（即所有）
            lrange mylist 0 -1

1.  `lpop`，`rpop`
    
    提取数据并从list中删除数据，表示从头部（`lpop`）或尾部（`rpop`）提取一个数据并从list中消除该条数据;如果list中没有数据时返回`null`

    *   使用格式
        
            lpop list

            rpop list

    *   ex
            
            lpop mylist

            rpop mylist
    
1.  `ltrim`
    
    设置list，使list只保存给定下标范围的数据，超出给定index的数据将被remove掉

    *   使用格式
            
            ltrim list index1 index2

    *   ex
            
            ltrim mylist 0 2

1.  `brpop`，`blpop `
    
    list的阻塞操作，常用来构建队列

    如果list为空，客户端将等待，指定其他客户端push数据或超时

    timeout设置为0时，客户端将一直等待直到其他客户端向list中push数据

    可以跟多个list，当第一个list收到数据时返回

    多个客户端同时执行阻塞操作时，将按先后顺序返回（先阻塞的先返回）

    正常（非timeout）返回值是一个由两个元素组成的数组

    超时将返回null

    *   使用格式

            brpop list1 list2 ... timeout

    *   ex
            
            brpop mylist 5

1.  `llen`
    
    获取list的长度

    *   使用格式
            
            llen list

    *   ex

            llen mylist

1. [List 的更多命令](http://redis.io/commands#list "More lists opertaors")

### Hash 常用命令

1.  `hmset`
    
    向Hash中设置对个field

    *   使用格式
        
            hmset hashkey field1 value1 field2 value2 ...

    *   ex

            hmset user:1000 username antirez birthyear 1977 verified 1


1.  `hget`
    
    查询单个filed

    *   使用格式
        
            hget hashkey field

    *   ex
            
            hget user:1000 username

1.  `hmget`
    
    查询多个field，返回一个数组（保存对应域的值）

    *   使用格式
            
            hmget hashkey field1 field2 ...

    *   ex
            
            hmget user:1000 username birthyear no-such-field

1.  `hincrby`
    
    针对单个field进行增长操作

    *   使用格式
        
            hincrby hashkey field step

    *   ex
            
            hincrby user:1000 birthyear 10

1.  [Hash 操作更多命令](http://redis.io/commands#hash "More Hash operators")


