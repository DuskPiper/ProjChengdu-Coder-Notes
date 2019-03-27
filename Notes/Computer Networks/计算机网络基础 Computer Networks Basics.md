# 计算机网络基础 Computer Networks Basics

## 概述

### ISP 互联网服务提供商

- ISP从互联网管理机构获得IP地址，拥有通信线路及路由器等联网设备，个人或机构向ISP购买服务就可接入互联网。

- 互联网是一种多层次ISP结构，ISP根据覆盖面积的大小分为第一层ISP、区域ISP和接入ISP。互联网交换点 IXP 允许两个 ISP 直接相连而不用经过第三个 ISP。

- IXP 互联网交换中心：不同电信运营商间为连通各自网络而建立的集中交换平台，一般由第三方中立运营。

  ![IXP & ISP](<https://camo.githubusercontent.com/b5bdb659b78196ccfa88909e4b9c8263d2af2cb0/68747470733a2f2f67697465652e636f6d2f437943323031382f43532d4e6f7465732f7261772f6d61737465722f646f63732f706963732f385f323030313535303831323033383139302e706e67>)

  

### 主机间的通讯方式

- Client - Server (C/S)：客户是服务的请求方，服务器是服务的提供方。
- P2P 对等网络：每个参与者具有同等的能力，可以发起一个通信会话。[详细](https://blog.csdn.net/ljianhui/article/details/16911151)

### 时延

时延 = 排队时延 + 处理时延 + 传输时延 + 传播时延

- 排队时延：分组在路由器的输入队列和输出队列中排队等待的时间，取决于网络当前的通信量。
- 处理时延：主机或路由器收到分组时进行处理所需要的时间，例如分析首部、从分组中提取数据、进行差错检验或查找适当的路由等。
- 传输时延：主机或路由器传输数据帧所需要的时间。
- 传播时延：电磁波在信道中传播所需要花费的时间，电磁波传播的速度接近光速。

## 体系结构

![layers](<https://camo.githubusercontent.com/307487dbeaf00492335d1f034529100e4f368a43/68747470733a2f2f67697465652e636f6d2f437943323031382f43532d4e6f7465732f7261772f6d61737465722f646f63732f706963732f66356434306230312d616266322d343335652d396365372d3738383963343162326661362e6a7067>)

### The 5-Layer Model (the TCP/IP Model)

- **应用层 Process & Application Layer**：为特定应用程序提供数据传输服务，数据单位为报文。协议代表：<u>HTTP、DNS、WWW</u> 等。

- **传输层 Transport Layer**：为进程提供通用数据传输服务。由于应用层协议很多，定义通用的传输层协议就可以支持不断增多的应用层协议。Unix把"主机+端口"称作Socket，由此产生两种协议：<u>TCP</u> 提供完整性服务，<u>UDP</u> 提供及时性服务。

  ​	**传输控制协议 TCP**，提供面向连接的可靠数据传输服务，数据单位为报文段；

  ​	**用户数据报协议 UDP**，提供无连接的尽最大努力的数据传输服务，数据单位为用户数据报。

- **网络层 Network Layer**：基于网络寻址协议为主机提供数据传输服务（而传输层为主机中的进程提供数据传输服务）。把传输层传递下来的报文段或用户数据报封装成分组。协议代表：<u>IP、IPX、RIP、OSPF</u>等。

- **数据链路层 Data Link Layer**：为同一链路主机提供数据传输（网络层针对的是主机间数据传输，而主机之间可以有很多链路）。数据链路层把网络层传下来的分组封装成帧。协议代表：<u>Eternet、MAC地址</u>等。

- **物理层 Physical Layer**：在传输媒介上传输数据比特流（0和1的电信号）。尽可能屏蔽传输媒介和通信手段的差异，使数据链路层感觉不到这些差异。协议代表：<u>电缆、光纤、双绞线、无线电波</u>等。

### The 7-Layer Model (the OSI Model)

OSI是Open System Interconnect的缩写，其将TCP的**应用层**拆分为三层功能：

- **会话层 Session Layer**：建立和管理进程间会话。
- **表示层 Presentation Layer**：数据压缩、加密以及数据描述，如ASCII，Unicode，BMP编解码。
- **应用层 Application Layer**：计算机操作及界面管理。



## 1 物理层 Physical Layer

###通信方式

根据信息在传输线上的传送方向，分为以下三种通信方式：

- 单工通信：单向传输；
- 半双工通信：双向交替传输；
- 全双工通信：双向同时传输。

### 带通调制

带通调制把数字信号转换为模拟信号。



##2 链路层 Data Link Layer









#### 

# References

- [Notes on the 5-Layer and 7-Layer Models of Interconnection](https://www.ischool.utexas.edu/~wyllys/ITIPMaterials/NotesOnInterconnection.html)
- [一张图了解TCP/IP五层网络模型](https://blog.csdn.net/zhshulin/article/details/62888061)
- [互联网协议入门（一） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)