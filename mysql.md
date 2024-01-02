### Mysql与Redis的区别

#### 一、两者的数据库类型

redis是NOSQL，非关系型数据库，也叫缓存数据库。将数据存储在缓存中，这虽然提高了运行效率，但是保存时间却很短。

mysql是关系型数据库，主要是用于存放持久化数据，将数据存储在硬盘中，但这样会造成读取速度慢。



#### 二，redis和mysql的运行机制

1.Redis是基于单线程的，Redis效率比较高，因为它是基于内存操作，所以CPU不是性能瓶颈，机器内存及宽带才是redis的瓶颈。

2.mysql数据库作为储存的的关系型数据库，它的弱点就是在每次请求访问数据库时，都存着I/O操作，如果频繁访问数据库会产生如下一些问题：

a.会在反复链接数据库上花费大量时间，从而导致运行效率过慢

b.反复的访问数据库也会导致数据库的负载过高，此时就引出缓存的概念

#### 三，那么什么是缓存数据库呢？

缓存就是数据交换的的缓冲区，当浏览器执行请求时，首先会对缓存中进行查找，如果存在就获取；否则就会访问数据库。

缓存的好处：最最最直观好处就是读取速度快。

redis的数据库就是一款缓存数据库，用于存储使用频繁的数据，这样减少访问数据库的次数，提高运行效率。



#### 四，Mysql和Redis各自的优缺点。
![1.png](https://github.com/yyds-zy/GoLang/blob/master/images/1.png?raw=true)
![2.png](https://github.com/yyds-zy/GoLang/blob/master/images/1.png?raw=true)

#### 五，最后redis和mysql的区别总结

1.数据库类型的区别

mysql是关系型数据库，Redis是非关系型数据库，缓存数据库。

2.作用上的区别

MySQL用于持久化存储数据到硬盘，功能强大，但是速度缓慢。
Redis用于存储使用较为频繁的数据到缓存中，读取速度快。

3.数据存储的位置区别

Mysql：数据存放在磁盘中，

Redis：数据存放在内存中。

4.存放的数据类型区别

Mysql：数值、日期、具体时间、字符串

Redis：String、Hash、List、Set、Zset

5.需求上的区别

mysql和redis因为需求的不同，一般都是配合使用。需要高性能的地方使用Redis，不需要高性能的地方使用MySQL。存储数据在MySQL和Redis之间做同步。



#### 六，数据可以全部直接用Redis储存吗？

我们将逐个分析。

1.MySQL存储在磁盘里，Redis存储在内存里。

Redis既可以用来做持久存储，也可以做缓存，而目前大多数公司的存储都是MySQL + Redis，MySQL作为主存储，Redis作为辅助存储被用作缓存，加快访问读取的速度，提高性能。

2.Redis存储在内存中，如果存储在内存中，存储容量肯定要比磁盘少很多，那么要存储大量数据，只能花更多的钱去购买内存，造成在一些不需要高性能的地方是相对比较浪费的，所以目前基本都是MySQL(主) + Redis(辅)，在需要性能的地方使用Redis，在不需要高性能的地方使用MySQL。

3.MySQL支持sql查询，可以实现一些关联的查询以及统计。

4.Redis对内存要求比较高，在有限的条件下不能把所有数据都放在Redis。

5.MySQL偏向于存数据，Redis偏向于快速取数据。但是Redis查询复杂的表关系时，不如MySQL，所以可以把热门的数据放Redis，MySQL存基本数据。