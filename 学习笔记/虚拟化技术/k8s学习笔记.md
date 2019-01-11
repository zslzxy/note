# Kubernetes学习笔记

**一切以服务为中心，一切围绕着服务运转。**

k8s 是以Docker为基础，打造的一个云计算时代的全新分布式系统架构。

k8s 底层支持的是 Docker以及Docker的竞争对手Rocket。

k8s 是一个一站式完善的分布式系统开发与支撑平台。



## 一、使用Kubernetes优势

​	Kubernetes是一个全新的基于容器技术的分布式架构领先方案，由Google开源而来。

​	Kubernetes能够很有效的避免底层代码和功能模块，避免项目的部署、实施，避免选型复杂的服务监控、治理框架。最主要是能够提供了强大的自动化机制，有效提高运作、维护能力，让更多经历投注于业务中。

​	Kubernetes是一个开放性质的平台，对各类语言编写的服务都可以轻松的映射为Kubernetes的service，通过标准的tcp协议进行通信操作。

​	Kubernetes是一个玩呗的分布式系统的支撑平台，能够支撑庞大的分布式项目。提供了完善的集群管理能力，包括多层次的安全防护策略、多租户应用的支撑能力、透明的服务注册机制，发现机制、负载均衡能力、故障自我修复，完善能力、简单扩容、资源粒度调配等操作。

### 1、Kubernetes核心--Service

​	在Kubernetes中，**Service（服务）是分布式集群架构的核心**，service的服务进程是基于Socket的通信方式，每一个Service通常是有一个或者多个服务组成的，每一个Service都有着独立的Endpoint（ip + port），能够根据这个IP地址和端口号，调用Socket进行连接。

​	一个Service拥有如下特征：

- 拥有一个唯一指定的名字。
- 拥有一个虚拟IP（Cluster IP，Service IP，VIP）和端口号。
- 能够提供某种远程服务的能力。
- 被映射到了提供这种服务能力的一组容器应用上。

### 2、Kubernetes隔离机制--Pod

​	**容器（Container）**能够提供强大的隔离功能，每一个Service提供服务的这组进程放置到一个容器中进行运行，实现隔离操作。Kubernetes中设计了 Pod对象，**将每一个服务进程包装到对应的Pod容器中**，使其成为一个在Pod中运行的容器。

​	在Kubernetes设计中，为了给**Service和Pod建立连接，需要在Pod中添加一个label标签**，这个label标签的形式为： 运行mysql的Pod --- name=mysql；

​	**Pod通常是运行在一个节点-Node中**(节点就相当于是一个物理机、公有云中的虚拟机等)；而通常情况下，一个节点中能够运行很多的Pod。

​	Pod中运行着 **一个特殊的 Pause容器和其他容器，其他容器能够共享Pause容器中的网络栈和Volume挂在卷**，这一特性能够保证他们的服务之间更加高效的进行通信和数据交互。

### 3、Kubernetes集群特征

​	Kubernetes集群管理中，划分为**一个主Master节点** 和 **多个从工作节点(Node)**。

​	**Master节点**主要是运行着集群管理相关的一组进程  kube-apiserver、kube-controller-manager和kube-scheduler，这些进程实现了整个集群的资源管理、Pod调度、弹性压缩、安全控制、系统监控和纠错等信息。

​	**从节点(Node)**在Kubernetes中是作为工作节点存在的，运行着的是真正的应用程序，在Node上Kubernetes管理的最小单元是Pod，Node中运行着kubelet、kube-proxy服务进程，这些服务进程负责的是Pod的创建、启动、监控等操作。

### 4、Kubernetes扩容

​	Kubernetes能够更加轻松的实现扩容操作。只需要**将扩容的Service关联的Pod创建一个Controller**(简称：CR)，即可轻松实现扩容。RC中包含的有：

- 目标Pod的定义。

- 目标Pod需要运行的副本数量（Replicas）。

- 要监控的目标Pod的标签（label）。

  当Controller创建好以后，服务器能够根据你创建的Controller中对应的label数量，来实现新的Pod的创建。实现自动化扩容操作。

# 看到了17页，第 1.2节