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

后来，随着ASIC(Application Specific Intergrated Circuit, 专用集成电路)技术的发展，ip路由使用硬件来查表了，mpls不再具备优势。

但是，MPLS的本质是一种隧道技术，天然可以充当多个业务的公网隧道。且MPLS依靠固定的标签交换路径，是一种面向连接的转发技术，使得其在流量工程(Traffic Engineering, TE)、Qos等领域也有广泛应用。

# MPLS有哪些特征
## 基本概念
### FEC

### MPLS标签

### 标签操作


### LSP

## MPLS网络是怎么样的

## MPLS有什么价值


# MPLS是如何工作的
## MPLS如何建立LSP

## 报文如何通过LSP转发

# 什么是MPLS VPN




>内容引用 https://info.support.huawei.com/info-finder/encyclopedia/zh/MPLS.html


