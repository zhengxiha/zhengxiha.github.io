---
title: jmeter压测插件
date: 2023-07-10 17:20:15
categories: #分类
- 性能测试
tags: #标签
  - jmeter


---
> > 前提：安装在[插件管理](https://jmeter-plugins.org/install/Install/)中下载jpgc--standard set
> 如果没有插件管理，到官网下载jar包后放在lib/ext目录下

![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1669343023103-d8375293-4f62-46f3-abf3-39ffb0ec45d1.png#averageHue=%233d4144&clientId=uca5deaf1-8454-4&from=paste&height=354&id=ubc75f6c3&originHeight=354&originWidth=533&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38213&status=done&style=none&taskId=u1fafdf64-9e2b-44a4-9800-fba77f671e3&title=&width=533)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1669343410292-3965a336-82b2-4451-a5d4-9fd2d74727f1.png#averageHue=%233e4143&clientId=ua6a74019-8f6f-4&from=paste&height=487&id=u873ea412&originHeight=487&originWidth=547&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31327&status=done&style=none&taskId=uc1db2fd9-adc2-4e65-9eeb-de7c81e0be1&title=&width=547)<br />一、两种配置界面<br />阶梯式加[压测](https://so.csdn.net/so/search?q=%E5%8E%8B%E6%B5%8B&spm=1001.2101.3001.7020)试（bzm - Concurrency Thread Group）<br />阶梯式使用场景：<br />该场景主要应用在负载测试里面**，**通过设定一定的[并发](https://so.csdn.net/so/search?q=%E5%B9%B6%E5%8F%91&spm=1001.2101.3001.7020)线程数**，**给定加压规则**，**遵循“**缓起步，快结束**”的原则**，**不断地增加并发用户来找到系统的性能瓶颈**，**进而有针对性的进行各方面的系统优化；<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1668394174221-b9d9e174-b6f5-4f78-a032-be92213b6ef8.png#averageHue=%236f7376&clientId=u55883303-1c0a-4&from=paste&id=u82d38930&originHeight=864&originWidth=1536&originalType=url&ratio=1&rotation=0&showTitle=false&size=112455&status=done&style=none&taskId=u1a47b082-00ba-4c1d-9abb-35f40ad4599&title=)<br />与其对应的是进步线程组（jp@gc - Stepping Thread Group），其配置页面如下图所示：该插件[jmeter](https://so.csdn.net/so/search?q=jmeter&spm=1001.2101.3001.7020)官方已经不再推荐使用了，所以在进行阶梯压测的时候可以选择 **阶梯式加压测试**（bzm - Concurrency Thread Group）<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1668394250182-aa78dadf-be38-450a-9fb7-583b5c214b4c.png#averageHue=%2374787b&clientId=u55883303-1c0a-4&from=paste&id=ud224700b&originHeight=864&originWidth=1536&originalType=url&ratio=1&rotation=0&showTitle=false&size=126015&status=done&style=none&taskId=uf81318f3-cc3a-4a15-a715-237877bdceb&title=)


