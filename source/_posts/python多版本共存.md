---
title: python多版本共存
date: 2023-04-28 17:20:15
categories: #分类
- 自动化测试
tags: #标签
  - python

typora-root-url: ./..
---
1、将不同版本放在不同的目录下<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658296439642-ed40f721-b46f-485f-b911-95587d908c55.png#averageHue=%23fbfbfa&clientId=u8ad53844-af4d-4&from=paste&height=256&id=u1a009db5&originHeight=320&originWidth=810&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23440&status=done&style=none&taskId=ucd9ac5a2-00fd-4eea-a42f-828a91bf32b&title=&width=648)<br />2、复制python.exe并重命名<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658296534075-1dec37ee-3d96-4fe6-abfd-d169a3f52467.png#averageHue=%23fbfaf9&clientId=u8ad53844-af4d-4&from=paste&height=459&id=uc3c1e253&originHeight=574&originWidth=749&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40977&status=done&style=none&taskId=u07bb7b03-e5d7-43d9-ba81-b16bbd2bfe9&title=&width=599.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658296555137-ffbbe867-0b35-4034-a3a4-7d6a12f40221.png#averageHue=%23f9f8f7&clientId=u8ad53844-af4d-4&from=paste&height=472&id=u88ba154d&originHeight=590&originWidth=648&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45513&status=done&style=none&taskId=ubf598a6a-d54f-4098-9be3-994924464ba&title=&width=518.4)<br />3、环境变量要把两个版本路径都放进去（哪个版本在前，cmd输入python，执行的就是最靠前的那个python版本，pip安装包也同理）<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658296670978-47ae0bc4-8d59-4f2a-86cc-6a6c1b628226.png#averageHue=%23f3f1ef&clientId=u8ad53844-af4d-4&from=paste&height=523&id=u00d6473a&originHeight=654&originWidth=702&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62639&status=done&style=none&taskId=ud904b188-4745-4a49-b33c-984dde99037&title=&width=561.6)<br />4、验证<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658296841623-85656b33-613f-4772-a6ab-029e30356760.png#averageHue=%23110f0e&clientId=u8ad53844-af4d-4&from=paste&height=286&id=u922eb459&originHeight=358&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46186&status=done&style=none&taskId=ud5234b2e-45ed-42db-8cb0-e8f767b402b&title=&width=832.8)

指定python版本安装库，使用python -m pip install xxx<br />如：在python36下安装requests
```
python36 -m pip install requests

#查看某版本安装了哪些库
python36 -m pip list
```
