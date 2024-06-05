---
title: arp攻击
date: 2023-07-22 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali

typora-root-url: ./..
---


需解决问题：
> 1、arpsoof是什么东西？
> 2、arp攻击、arp欺骗、arp缓存表是什么？
> 3、反向欺骗是什么？
> 4、如何用arp攻击实现断网？
> 5、如何做arp攻击防御？

<a name="S81XL"></a>
### 1、arpsoof是什么东西？
> arpsoof是一款进行arp欺骗的工具，攻击者通过**修改受害者arp缓存**，将网关的MAC替换为攻击者的MAC，后续攻击者可截获受害者的所有发送和接收的所有数据包

![img](/images/img.png)
<a name="cLZMD"></a>

### 2、arp攻击、arp欺骗、arp缓存表是什么？
arp缓存表：
> 每台主机、网关都有一个ARP缓存表
> ARP缓存表内记录了其他主机或网关的IP地址与MAC地址的对应关系
> 表中的每条记录实际上就是一个IP与MAC地址对，可以是静态的，也可以是动态的；
> 静态的记录不能被ARP应答包修改，动态的可被ARP应答包修改

arp攻击：无法通信<br />arp欺骗：窃取数据
```python
相关命令：
arp -s 增加
arp -d 删除
arp -a 查看缓存表  #这个在windows中可以查看是静态还是动态，linux中只有对应的记录
netsh interface ipv4 show neighbors   #windows中查看缓存表
netsh interface ipv4 set neighbors 网卡id 目标ip 目标mac  #绑定arp
```
<a name="SUXku"></a>
### 3、反向欺骗是什么？
```python
# 正向欺骗
arpsoof -i 网卡 -t 目标主机ip  目标主机网关ip
# 反射欺骗
arpsoof -i 网卡 -t 目标主机网关ip  目标主机ip
```
![img_1](/images/img_1.png)![img_2](/images/img_2.png)
<a name="Q1DCO"></a>

### 4、如何用arp攻击实现断网？
开启arp攻击后不开启路由转发，这样目标机的流量只发到攻击者的主机，不会转发到网关，也就无法得到回应，实现断网
```python
# 两种命令 其中1表示开启 0表示关闭
# 第一种
sysctl -w net.ipv4.ip_forward=1
# 第二种
echo 1 > /proc/sys/net/ipv4/ip_forward   

# 查看路由转发状态
cat /proc/sys/net/ipv4/ip_forward 

```
<a name="weMKu"></a>
### 5、如何做arp攻击防御？
法一：将网关的mac地址静态绑定（不太现实，必须主机和网关都绑定）<br />方法：
> 1、查看网络接口信息

```python
netsh interface ipv4 show interface
```
> 2、将网卡绑定

> **（1）静态绑定ARP，真实环境不太现实 必须在主机和网关双向绑定**
>         -----主机上绑定-----
>         先查看网卡的id
>         netsh interface ipv4 show neighbors
>         再进行绑定
>         语法：netsh interface ipv4 set neighbors 网卡id  目标主机ip地址  目标主机mac地址
>         示例：netsh interface ipv4 set neighbors 11 10.0.0.178 00-1a-e2-df-07-41
>         如果是xp系统用：arp  -s  目标主机ip地址  目标主机mac地址
>  	-----网关路由器上绑定-----
>         语法：Router(config)#arp 目的主机ip地址 目标主机mac地址 arpa 接口
>         示例：Router(config)#arp 10.0.0.95 0013.240a.b219 arpa f0/0
>         -----交换机上绑定-----
>         语法：Switch(config)#arp 目的主机ip地址 目标主机mac地址 arpa 接口
>         示例：Switch(config)#arp 10.0.0.12 90fb.a695.4445 arpa f0/2
>     查看arp缓存表：arp  -a
>     清除arp缓存表：arp  -d
> **（2）安装ARP防火墙，或企业级防火墙（自带ARP防火墙功能）**
> 原文链接：[https://blog.csdn.net/lzl10211345/article/details/126882928](https://blog.csdn.net/lzl10211345/article/details/126882928)

法二：安装ARP防火墙，或企业级防火墙（自带ARP防火墙功能）
