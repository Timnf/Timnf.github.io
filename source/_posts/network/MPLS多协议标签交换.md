---
title: MPLS(Multi-Protocol Label Switching)多协议标签交换
tags: 网络路由
categories: 网络
---

# 什么是MPLS
**MPLS是一种在骨干网上利用标签来指导数据报文高速转发的协议**，相对于传统ip路由方式，提供了一种新的网络交换方式，**它将ip地址映射为简短且长度固定、只具有本地意义的标签，以标签交换替代ip查表，从而显著提升转发效率**。同时该标签机制可以再ip网络中构筑逻辑上的隧道，因此可以为各种L2VPN、L3VPN及EVPN等业务提供公共隧道服务。

# 为什么需要MPLS
起初硬件技术限制，基于最长匹配算法的IP技术必须使用软件方法查找路由，转发性能低下。**MPLS最初就是为了提升IP网络中路由设备的转发速率**。

![](../../image/network/mpls/mplsVSip.png 'IP路由与MPLS方式对比')


通过两种方式提升转发速率:
|MPLS方式|IP路由方式|
|------|------|
|简洁的标签交换|庞大的路由表|
|中间节点无需解析IP报文头|每个节点都需要解析|



```plantuml
@startmindmap
* Root
** Node 1
*** Child 1
*** Child 2
** Node 2
*** Child 3
**** Grandchild
@endmindmap
```


```mermaid
graph TB
    A(接口请求) --> B[参数校验]
    B[参数校验] --> C{校验通过?}
    C{校验通过?} -- 通过 --> d[处理业务逻辑]
    C{校验不通过} -- 不通过 --> e[结束]
    d[处理业务逻辑] --> e(结束)
```



# MPLS有哪些特征


