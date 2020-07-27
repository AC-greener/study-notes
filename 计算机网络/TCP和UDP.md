### TCP和UDP

![1569392573109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569392573109.png)

### TCP

![1569417691884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569417691884.png)

1. TCP是面向连接的（确认通信端存在时才会发送数据），可靠的流协议。可以把流想象成排水管道中的水流，比如：发送端发送了10次100字节的的信息，接收端有可能收到一个1000字节连续不间断的数据

2. 丢包重发

3. 序列号和确认应答。 丢包分为两种：A向B发送时数据真正丢包，B确认应答时数据包丢了，导致A没有收到确认，从而进行重发

   ![1569394857154](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569394857154.png)

   ![1569394727800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569394727800.png)

4. 超时重发

5. 连接管理

   一个连接的建立和断开至少需要七个包

   ![1569395061562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569395061562.png)

6. 利用窗口控制提高速度

   之前是为每个数据包进行确认应答，包的往返时间越长，通信性能越低，如下图：![1569396974953](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569396974953.png)

   

   现在增加了滑动窗口，确认应答不在是以每个包，而是以更大的单位进行确认

   ![1569397139176](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569397139176.png)

   每个窗口内的数据即使没用收到确认应答也可以发送数据。

   收到确认应答时，会将窗口滑动到确认应答的序列号位置

   ![1569397358258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569397358258.png)

   如何控制和重发控制：

   1，考虑确认应答未能返回的情况：使用了滑动窗口后，确认应答丢失也无须重发

   ![1569398211186](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569398211186.png)

   2，考虑真正的数据包丢失。发送主机如果三次收到同一个确认应答，就会将其对应的数据进行重发

   ![1569398320887](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569398320887.png)

   

7. 流控制

8. 拥塞控制

   在网络发生拥堵时，如果突然发送一个过大的数据，极有可能导致网络瘫痪，为了防止该问题，在通信一开始会通过一个慢启动的算法对发送的数据量进行控制

   为了在发送端调节所发送数据的量，定义了拥塞窗口这个概念，刚开始拥塞窗口的大小为1个数据段，之后的每一次确认应答，窗口的值就会增大。在发送数据包时，会将拥塞窗口的大小与接收端主机通知的大小作比较，然后取较小的那个值，发送比这个值还要小的数据量

   **慢开始算法的思路**：由于并不清楚网络是否拥塞，比较好的方法是先谈测一下，也就是由小到大逐渐增加发送窗口

### 提高网络利用率的方法

1. Nagle算法，目的是为了减少网络上较小的数据包从而减小网络拥塞的出现

   该算法要求一个tcp连接上最多只能有一个未被确认的未完成的小分组，在该分组ack到达之前不能发送其他的小分组，tcp需要收集这些少量的分组，并在ack到来时以一个分组的方式发送出去；其中小分组的定义是小于MSS的任何分组

   发送端即使还有要发送的数据，但如果这部分数据很少的话，则进行延迟发送的一种机制

2. 延迟确认应答



### UDP

![1569417731173](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1569417731173.png)

1. 使用场景：主要用于高速传播和实时性要求较高的通信
2. 不可靠
3. 没有拥塞控制，出现丢包也不重发，当包的顺序乱掉时，也没有纠正的功能



端口号用来识别一个电脑中不同的应用程序