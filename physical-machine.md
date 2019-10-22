# physical-machine

**简介**

​	真实环境中测试程序性能的方法及注意事项。

**物理机**

​	服务器两台(node5 & node6)，每台各有4个网卡，千兆网卡、万兆网卡各两个。

​	```lspci | grep -i eth```命令可查看网卡详情。

**网卡设置**

​	每台服务器有一个网卡(eno1)用于通信，剩余3个网卡(eno2, eno3, eno4)与另一台服务器对应直连，用于实验。网口连接示意图如下：

![alt](https://raw.githubusercontent.com/miigao/misc/master/network-topology.png)

