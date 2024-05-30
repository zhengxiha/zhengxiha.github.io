---
title: VMware-Kali网络设置
date: 2023-05-04 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali


---



1、桥接模式-适合设置与主机相同网段<br />2、NAT模式-适合设置与主机不同网段------这里我试过设置与主机同网段，但ip一直无法配置成功<br />3、仅主机模式-只与主机通信，不能与外网通信

流程：<br />1、设置虚拟机通信模式<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682573064373-06af4029-cd8b-4786-8f4e-02f675492407.png#averageHue=%23f4f4f3&clientId=u8e0517f7-93ec-4&from=paste&height=543&id=u86708d06&originHeight=543&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37032&status=done&style=none&taskId=u3ff15ef4-155d-47b3-8864-eacf8d4bdaa&title=&width=876)<br />2、查看主机ip dns 网关信息<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682573152019-8f183f43-b17d-4451-bcf4-f3c90cabfbd0.png#averageHue=%23e4e3e2&clientId=u8e0517f7-93ec-4&from=paste&height=711&id=u9ee36cf4&originHeight=711&originWidth=1139&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105094&status=done&style=none&taskId=uf6d2b77a-ef92-47bb-b5e7-f0456d50ce4&title=&width=1139)<br />3、按模式进行更改<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682573280114-674a8b3d-ef46-420c-be4f-f8ea2f9375bc.png#averageHue=%23dbe2be&clientId=uee4ee051-da7b-4&from=paste&height=853&id=u2bf3b40b&originHeight=853&originWidth=1462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=201870&status=done&style=none&taskId=uad3ce1bb-6801-457b-a25c-682fc8eda09&title=&width=1462)<br />4、进入虚拟机内配置
```
vim /etc/network/interfaces
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682573505272-3922ec73-ae55-4e30-a5ab-865a18aac632.png#averageHue=%23242d3b&clientId=uee4ee051-da7b-4&from=paste&height=510&id=u271a5605&originHeight=510&originWidth=650&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87335&status=done&style=none&taskId=u899bc6e4-07f1-4b55-9bdc-cb4fa0beb57&title=&width=650)
```
vim /etc/resolv.conf  #配置dns 
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682575821143-21c9a374-6114-43f1-992c-349315001445.png#averageHue=%23232631&clientId=uee4ee051-da7b-4&from=paste&height=495&id=ude4d5c83&originHeight=495&originWidth=617&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41251&status=done&style=none&taskId=ud10bfba3-ec43-4c4e-b628-ffefb4afcea&title=&width=617)<br />以上两个文件配置完后，依次执行以下命令
```
systemctl stop NetworkManager   #关闭并设置为开机不启动
systemctl disable NetworkManager		#设置为开机不启动
systemctl restart networking         #重启kali网络服务
ip a          #查看ip是否配置成功
```
