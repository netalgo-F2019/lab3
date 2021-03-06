# lab3

**简介**

* 这里给出了实验三的参考代码框架，可以以此为基础完成实验三的编程部分。
* server-main.c文件是唯一需要编辑的文件，你需要补充相关代码。

**代码结构**

​	server-main.c与实验二中的代码比较，主要的区别如下：

* 多包含了一个本地头文件SimpleDNS.h，该头文件包含了原生SimpleDNS中所有被调用的子函数。
* 原生SimpleDNS代码中的main函数部分转移到了server-main.c中的lcore_main函数中。

**注意事项**

* 为保障代码可读性，lcore_main函数主循环中与数据包收发无关的代码被进一步划分成数个代码块，它们介于两个注释行之间，如下：

  ```pseudocode
  /**********xxxxxx(begin)**********/
  {代码块}
  /**********xxxxxx(end)**********/
  ```

  原则上，既不需要修改这些代码块，也不需要在其中插入其它代码。

  若此限制对你的编程造成困难，可酌情修改，注意代码可读性。

* 相比于实验二中的build_udp_packet函数，实验三的build_packet函数需要实现更为具体的功能，

  以下给出一个参考实现方式：

  * 接受3个参数，两个指针分别指向查询包和应答包的协议头，第3个参数为应答包大小。
  * 利用查询包协议头中的信息，为应答包协议头中的数据项赋值。
  * 策略：从哪来，回哪去。MAC地址、IP地址、端口号直接交换。
  * build_packet函数只处理协议头。

* 分析原生SimpleDNS可知，应答包的大小在写入应答数据后才得出，所以以下几个步骤的相对顺序不能被颠倒：

  申请应答包mbuf→写入应答数据→写入协议头→发出应答包。

  如果你用了以上给出的参考实现，最后别忘了**释放查询包占的空间**。

* 最简单的实现是，每次循环只处理一个查询请求。

  (**update:2019/11/05**)：在目前用于实验的服务器上，如果程序设计为每次循环只处理一个查询请求，则不能正常运行(收不到包)，但程序在虚拟机或其它服务器上能够正常运行，这造成了一些困扰。为避免问题，请将程序实现为 一次接收多个包(推荐值：32) → 逐个解析 → 一次发送多个包 这样的批处理模式。

**验证**

与验证原生SimpleDNS的方法类似，使用dig命令进行验证，将IP地址改为虚拟机IP即可。