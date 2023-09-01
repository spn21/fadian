​	





## Sigma协议

设关系 $R$⊆$X$$*$,那么<P, V> 构建在R上的一个Sigma 协议为：

P是一个叫证明的交互式协议，其输入为一个witness-statement对($x$,$y$)∈$R$.
V是一个叫验证的交互式协议，其输入为一个statement，$y$∈$R$.

P和V交互过程为：

1. 首先，P计算一个承诺(commitment) t ，将其发送给V；
2. 在收到来自P的消息后，V在有限的挑战空间C中随机选取一个挑战元素(challenge) c，并将其发送给P ；
3. 在接收到来自V的挑战后， P计算出一个反馈(response) z ,将其发送给V
4. 在收到了来自P的反馈后, V输出accept或者reject。 $V$的输出由statement$y$和交互中的消息 $conversation(t,c,z)$严格计算得出。

协议的要求是，对于所有的$ (x,y)∈R $，当 $P(x,y) $与 $V(y) $交互后， $V(y)$ 总是输出accept。需要注意的是，为了系统的安全性，挑战空间的大小应该为sup-poly（超过多项式大小）

![](E:\fadian\sigma1.webp)

**Knowledge soundness**

设 $(P,V)$ 是关于关系 $R⊆X×Y$ 的一个Sigma协议。如果存在一个高效的确定性的算法$Ext$（称作一个**witness extractor**）使得：给定一个statement $y$ ，以及两个关于 $y$ 被accepted的conversation $(t,c,z)$ 和 $(t，$$$c^t$$,$z^t)$ ，并且 $c≠ c^t$，  $Ext$能够输出 $x∈X$ 使得$(x,y)∈R$（i.e.， $x$是 $y$ 的witness）则我们称 $(P,V)$ 提供 **knowledge soundness**。

**Definition 4: (Special honest verifier zero knowledge)**

设 $(P,V)$ 是关于关系 $R⊆X×Y$的一个Sigma协议，且挑战空间为 $C$ 。如果存在一个高效的概率性算法 $Sim$（称作**simulator**），其输入为$ (y,c)∈Y×C$ 且满足：

1. 对于所有的输入$(y,c)∈Y×C$ ， $Sim$能够输出一对 $(t,z)$ 使得$(t,c,z)$ 对于 $y$ 是一个被accpeted的conversation。
2. 对于全体$(x,y)∈R$，我们计算

$c\stackrel{R}{\longleftarrow}C,\ \ \ \ (t,z)\stackrel{R}{\longleftarrow}Sim(y,c)$

使得$(t,c,z)$的分布同$P(x,y)$ 和$V(y)$之间conversation的分布相同，则我们称$(P,V)$是**special HVZK。**Special的原因在于：1）$Sim$ 需要 �$c$作为额外输出；2）就算 $y$的witness不存在，$Sim$也可以输出一个被accepted的conversation。



### EIFFeL: Ensuring Integrity for Federated Learning

abstract:形式化确保FL中更新隐私和完整性问题，提出了一个新的系统EIFFel，可以强制执行任意的完整性检查，并从聚合中删除格式错误的更新且不会侵犯隐私

### Keywords

Poisoning Attacks, Input Integrity, Secure Aggregation

### 1.Introduction

FL一个主要问题：联邦学习者如何在不侵犯客户隐私的情况下有效验证客户更新的完整性

提出SAVI（secure aggregation of verified inputs)协议

1）安全验证每个本地更新的完整性

2）只聚合良好的更新

3）只发布最终的聚合

SAVI协议允许在不观察更新的情况下检查更新的格式是否正确，从而确保更新的隐私性和完整性

EIFFel：一个实例化SAVI协议的系统，更新检查其完整性确保对拜占庭攻击的鲁棒性。                

+优化方案，提升性能



### 2 Problem Overview

##### 2.1

FL中两种参与者，全局模型记为$M$

1.客户端CLient   (n个客户端$C_i$；私人数据集$D_i$)

2.服务器Server   (不受信任的服务器$S$：协调不同客户端更新训练$M$)

FL中单个训练迭代包括：

1.广播：服务器$S$向所有客户端广播$M$的当前参数

2.局部计算：每个$C_i$本地计算更新记为$u_i$，在数据集上表示为$D_i$

3.聚合：S收集$C_i$更新并聚合。U=$\Sigma{u_i}$

4.全局更新：$S$基于$U$更新$M$

##### 2.2安全目标

1.输入隐私（对客户端）

2.输入完整性（对服务器）

##### 2.3威胁模型

1.恶意服务器：偏离协议

2.恶意客户端（$C_m$）

​	（1）发送错误输入，影响最终聚合

​	（2）使诚实客户端的完整性检查失败

​	（3）侵犯诚实客户隐私（可能与服务器串通）

##### 2.4解决方案

SAVI协议（带验证输入的安全聚合）

EIFFeL为任何$Valid（.）$实例化SAVI协议，具有公共参数的算术电路，通过使用Shamir的阈值秘密共享方案确保输入隐私；输入完整性通过SNIP和VSS（可验证秘密共享）保证（4.1）

### 3.具有验证输入的安全聚合

定义：给定公共验证词$Valid(.)$和安全参数$k$，协议$Π(u_1,...,u_n)$为SAVI协议

诚实：客户端都有格式良好的输入

隐私：对于恶意客户端$C_M$和恶意服务器$S$

Figure2

remark1：默认服务器为诚实

remark2：防止拒绝服务攻击

### 4.EIFFeL系统描述

##### 4.1加密构建块

算术电路：$C:F^k ->F$  使用有限域加乘和常数乘法

Shamir’s 𝑡-out-of-𝑛 Secret Sharing Scheme：Schamir协议    

密钥协商协议：三种算法元组组成：

​	1.参数生成算法

​	2.密钥生成算法

​	3.密钥协议

认证加密：

​	1.输出私钥生成

​	2.加密密钥＋消息

​	3.解密算法（错误返回）				

秘密共享非交互式校对：SNIP为多验证器设置而设计（私有数据在验证器之间分发或秘密共享）

##### 4.2系统构建块

公共验证谓词$Valid(.)$：由算术电路表示

公共公告板：用于广播

##### 4.3设计目标

灵活兼容高效

##### 4.4工作流

在FL中SAVI安全聚合，用$Valid(.)$验证，SNIP检查客户端更新完整性，使$Valid(u)$=1.

main ideas：

​	1.客户端充当彼此验证器

​	2.恶意验证器也可以执行验证，使用Shamir’s 𝑡-out-of-𝑛 threshold方案

（fig4.完整协议）



### 5.安全性分析

定理1.$|C_M|<{(n-1)/3}$  	$C_valid=C/C^*$

验证草图基于

1.任意一个$m$或更少的EIFFel分享没有透露秘密

2.
