---
title: jmeter负载测试中找到最大并发用户数
date: 2023-02-23 17:20:15
categories: #分类
- 性能测试
tags: #标签
  - jmeter


---



<a name="zrgqi"></a>
#### 脚本命令行执行：
```java
jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder]

参数说明：
	[jmx file] 为测试计划文件路径

	[results file]  为测试结果文件路径

	[Path to web report folder] 为web报告保存路径。
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1668656386642-307fdb56-37d1-48eb-820d-39bdcede98f8.png#averageHue=%23313233&clientId=uac3beba7-eb78-4&from=paste&height=531&id=ua9844afd&originHeight=531&originWidth=1005&originalType=binary&ratio=1&rotation=0&showTitle=false&size=161652&status=done&style=none&taskId=udf5c6b8d-d048-487c-9bc9-85f023b1c00&title=&width=1005)

在性能测试中，当我们接到项目任务时，很多时候我们是不知道待测接口能支持多少并发用户数的。此时，需要我们先做负载测试，通过逐步加压，来找到最大并发用户数。那么当我们找到一个区间，怎么找到具体的值呢？

在区间中逐步增加步长，出现以下任意现象时，即是最大并发用户数：<br />**1.出现连续报错**<br />**2.平均响应时间超过1.5秒（1.5秒是行业标准）**<br />**3.tps出现下降趋势**

负载测试概念<br />逐步增加并发用户数，找出被测系统的最大可接受的并发用户数，并考察系统性能的变化。

注：并发量过大的时候最好不要添加聚合报告，可能会导致jmeter软件出错

[<br />](https://blog.csdn.net/begefefsef/article/details/126034230)

