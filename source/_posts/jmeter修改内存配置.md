---
title: jmeter修改内存配置
date: 2023-02-21 17:20:15
categories: #分类
- 性能测试
tags: #标签
  - jmeter


---


一、场景<br />问题：大数据、高并发压测过程中，出现jmeter卡死<br />显示：java.lang.OutOfMemoryError: Java heap space<br />原因：[jmeter](https://so.csdn.net/so/search?q=jmeter&spm=1001.2101.3001.7020)默认内存MaxMetaspaceSize=256m，运行[jmeter](https://so.csdn.net/so/search?q=jmeter&spm=1001.2101.3001.7020)机器的内存，占用较高，超过了jmeter设置的内存上限，内存溢出。<br />默认大小是不够的，我们可以设置MaxMetaspaceSize=1024m 或者2048m 

二、修改jmeter内存配置（win）<br />1、查看jmeter安装路径
```
which jmeter
```
2、**修改/apache-jmeter-5.4.3/bin/ 目录下的 jmeter文件**<br />2.1 查看电脑内存<br />电脑右键“属性”查看电脑内存

2.2修改jmeter内存配置<br />将jmeter文件（windows修改jmeter.bat文件），下面这一行修改为自己电脑的内存大小（建议修改为1024的倍数 1024=1G）

2.3找到jmeter.bat文件，找到![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1676626836929-f174665d-6429-4990-bac1-1cd6bd47debe.png#averageHue=%23f7f4f2&clientId=ud1b71ffc-04ae-4&from=paste&id=u02a87b75&originHeight=121&originWidth=1138&originalType=url&ratio=1&rotation=0&showTitle=false&size=20221&status=done&style=none&taskId=u52e6954c-64e4-4d6c-889f-72453f8d4e8&title=)<br />修改最后的数值（如1024）
```
 set HEAP=-Xms1g -Xmx1g -XX:MaxMetaspaceSize=1024m
```
<br /> 2.3确认是否生效<br />cmd输入jconsole.exe<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1676626769782-827a2e44-f65d-435c-8e91-ce11fa158207.png#averageHue=%23d6d4d3&clientId=ud1b71ffc-04ae-4&from=paste&id=ue423a437&originHeight=476&originWidth=892&originalType=url&ratio=1&rotation=0&showTitle=false&size=90594&status=done&style=none&taskId=u167cd436-21e1-4202-9f61-e11a15a43e7&title=)![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1676626778968-2a60d117-06d2-4f80-99a9-876efab66f5d.png#averageHue=%23f2f1f0&clientId=ud1b71ffc-04ae-4&from=paste&id=uff90be20&originHeight=521&originWidth=656&originalType=url&ratio=1&rotation=0&showTitle=false&size=137643&status=done&style=none&taskId=u3de09fdf-85ae-40d9-bd67-1df152dbffe&title=)
