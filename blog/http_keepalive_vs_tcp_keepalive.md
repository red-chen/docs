# HTTP KeepAlive vs TCP Keepalive

对于大多数人来说，都听过KeepAlive，但是大部分同学分不清楚HTTP和TCP的KeepAlive的区别。这个说有多难，其实也不难，网上也有很多文章介绍两者的区别，但是大部分就从配置和源码来讲，而且内容大同小异。所讲的内容也仅仅局限在功能的区别上，并没有与我们实际的生产工作内容直接挂钩，导致大家过目而忘。

我今天先给大家普及一下两者的基本概念，然后结合实际生产中大家常问的一些问题，给出解答，并给出每种场景下的TCP抓包信息。

# 基本概念

HTTP KeepAlive：这里有一个更好的表述：“是否复用已经建立好的连接”，英文用Reuse或者Multiplex更好，表示HTTP请求(Request&Response)是不是可以复用一个已经建立的TCP连接。

TCP KeepAlive：表示TCP是否开启心跳探测功能，保持一个TCP连接的持续存在

# HTTP KeepAlive 

### 1. 我们要开启HTTP KeepAlive的功能时，为什么只需要在HTTP服务器端(比如Nginx.conf)配置即可，客户端不用配置吗？
HTTP 1.1 默认是开启HTTP KeepAlive

### 2. 那我作为客户端，能主动关闭HTTP KeepAlive功能吗？

### 3. HTTP KeepAlive的3个场景的参数有什么作用？

### 4. 在HTTP KeepAlive模式开启的条件下，TCP 连接到达上限之后，服务端是怎么关闭连接的？

### 5. 在HTTP KeepAlive模式关闭的条件下，在请求完成之后，服务端是怎么关闭连接的？

### 6. 在HTTP KeepAlive开启的场景下，为什么HTTP服务器要关闭已经创建好的连接呢？

# TCP Keepalive

### 例子
1. 另外一端崩溃
2. 另外一端崩溃并重启
3. 另外一端不可达

### 1. TCP是怎么保持连接的？

### 2. 怎么设置TCP连接保持
