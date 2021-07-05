---
layout:     post
title:      Redis中有序列表Zset相关命令
subtitle:   Zset相关命令和时间复杂度
date:       2021-07-05
author:     HH
header-img: img/post-bg-map.jpg
catalog: true
tags:
    - Redis
    - 语言基础
---
## Redis中有序列表Zset相关命令

Redis有序集合和集合set是一样内部value为string类型的结合，有序不允许重复元素，但是zset的每个元素都有一个double类型的分数score。redis正是靠这个分数对元素从小到大排序。

zset中元素唯一，但是分数可以重复。

### 增

#### zadd

```
zdd key score value [score2 value2]
```

往zset中添加一个或多个元素。

```
127.0.0.1:6379> zadd student 1264 jiawenhao
(integer) 1
```

> 如果元素之前已经存在，那么相当于更新分数，那么如果分数也和之前一样相当于什么都不做。

### 查询

#### zcard

```
zcard key
```

获取有序集合内部的成员数（所有timeline）

```
127.0.0.1:6379> zcard student
(Integer) 4
```

#### zcount

```
zcount key min max
```

计算有序集合指定分数区间的成员数（生成时间区间）

#### zlexcount

```
zlexcount key min max
```

在有序集合中计算指定字典区间的元素数量。当有序集合的元素出现相同的分值时，有序集合会根据成员的字典序（lexicographical ordering）来进行排序，而这个命令则可以返回给定的有序集合键key中，值介于min和max之间的成员个数。

时间复杂度O(log(N))，其中N为有序集合的元素数量。

#### zrank

```
zrank key value
```

查询值在有序列表中的索引位置（在一屏中位置）

#### zrange

```
zrange key start end
```

通过索引区间返回该区间的元素（返回最近的或者最远的几条）

```
127.0.0.1:6379> zrange zset 0 1
1) "a"
2) "b"
```

### 修改

#### zincrby

```
zincrby key increment value
```

指定元素的分数自增，increment为增量（调整某个id的时间戳）

### 删除

#### zrem

```
zrem key value [value2]
```

移除有序集合中一个或者多个元素（删除指定的uid中几个id）

#### zremrangebyrank

```
zremrangebyrank key start stop
```

根据分数排名移除元素（排序属性，最近几条，或者最远几条）

```
127.0.0.1:6379> zrange zset 0 1
1) "a"
2) "b"
127.0.0.1:6379> zremrangebyrank zset 0
(Integer) 1
```

#### zremrangebyscore

```
zremrangebyscore key min max
```

根据分数范围去移除元素（时间属性，几月几号到几月几号）