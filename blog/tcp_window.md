# TCP协议: Advertised Window, Congestion Window 

Sliding Window(滑动窗口)由Advertised Window(通过窗口)和Congestion Window(拥塞窗口)组成，分别用于流量控制和阻塞控制。

## Advertised Window
Advertised Window（通告窗口），是指接收方最大能接收的数据大小，如果发送方发送的数据超过这个值，有可能数据会被丢弃。所有在我们的TCP实现中，发送方会严格准守这个规则。到Window size为0时，发送方会通过Persist Timer机制，周期性的探测（发送Window Probe）接收方的Window size是否依然为0，如果为0 ，则发送方不会发送数据。如果不为0，则继续发送数据。

TODO TCPDump图
TODO TCPDump图

## Congestion Window

### Slow start(慢启动)
### Congestion avoidance(拥塞避免)



### 引用
- [TCP流量控制中的滑动窗口大小](https://www.zhihu.com/question/48454744)
