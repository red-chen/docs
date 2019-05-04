# TCP协议: MSS与网络层的MTU

##  MTU 与 MSS

TODO 直接上图

### MTU
Maximum transmission unit, 属于Network layer(网络层)。我们常见的以太网（Ethernet）单帧(Frame)在64 ~ 1518 Byte，小于或者大于这个限制的，都视之为错误的数据帧。而MTU的大小一般有Link layer设备决定。比如最大的帧是1518 Byte，按照《TCP/IP Illustrated Volumn 1》的3.2.2的Ethernet Frame定义，Link layer有14个字节的和Header和4个字节的Trailler组成，所以Network layer最大只能传输1500 Byte（1518 - 18），这个就是MTU。

Ethernet MTU
```
+-------------+------------+-----------------+---------+----------------+
| Dest MAC(6) | Src MAC(6) | Eth Type/Len(2) | Payload | CRC Trailer(4) |
+-------------+------------+-----------------+---------+----------------+
```

这里还有一个概念，就是 Path MTU（路径MTU），简单定义就是在整个传输链路设备上最小的MTU。

### MSS
Maximum segment size, TCP数据包每次能传输的最大有效数据分段，计算方式是MSS = MTU - 20byte(IP协议头) - 20byte(TCP协议头) 

MSS在TCP连接创建的时候有Client和Server端协商决定的，通信双方会选取最小的MSS最为这次连接的最大MSS值。

### 最重要的问题
1. 为什么Network layer有MTU后，Transmission layer还有MSS呢？

MTU和MSS的功能是一样的，只是在不同“层”上对包大小进行分片，最终表现的结果大不一样。

Network layer提供的是不可靠传输方式，在网络层上，任何一个包在传输过程中丢失，网络层是无法发现的，需要靠上层应用来保证。比如一个IP包，被切分为多个包传输，任何一个子包丢失，上层应用会发现传输失败，但是上层应用无法判断也无法发起仅重传丢失的分片，最终发起整个包的重新传输。结论就是IP包越大，重传大家越高。

TCP协议提供了一个可靠的传输方式，对于TCP协议来说，TCP自身已经实现了重传机制，任何丢失的TCP数据包协议都能发现并且单独重传，所以TCP为了传输效率，防止被IP协议分片，就有了MSS。MSS如上面公式所知，MSS的值就是根据MTU计算所得，保证了数据不被分片，有保证了最大的传输效率。

### 引用
- [什么是 MTU, 什么是 MSS](http://blog.crhan.com/2014/05/mtu-and-mss/)
