---
title: 目录扫描工具-dirb、dirbuster
date: 2023-05-06 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali
typora-root-url: ./..
---

<a name="ymqfO"></a>

# 一、kali自带扫描工具-dirb 
<a name="llkVi"></a>
## 1、简介
> dirb是一个基于字典的Web目录扫描工具（kalI自带），采用递归的方式去获取更多的目录；
> 可以查找到已知的和隐藏的目录；
> 支持代理，延迟访问和添加请求头。

<a name="SiMTP"></a>
## 2、使用
大多数情况下，如果对一个网站的语言不清楚，可以直接使用dirb进行扫描，但访问过多会被封IP，所以最好使用时**设置参数**
<a name="mEknu"></a>
##### 直接使用：
```python
dirb 域名/主机ip
示例：
dirb http://dc-2/
dirb http://114.67.175.24:17896
```
<a name="brycL"></a>
##### 加参数：
| 参数 | 作用 |
| --- | --- |
| -a | 设置User-Agent |
| -p | proxy，设置代理 |
| -c | cookie，设置cookie |
| -z | 设置访问间的延迟，单位毫秒（可以避免ip被封） |
| -o | oupput，可以控制输出结果到指定的路径文件中 |
| -X（大写） | 如果已经确定要扫描的文件后缀，可以使用此参数，直接加上后缀（某种程度上可以减少扫描） |
| -i | 不区分大小写 |
| -H | 添加请求头 |

```python
# 使用/usr/share/wordlists/dirb/big.txt字典来扫描Web服务
dirb https://www.baidu.com /usr/share/wordlists/dirb/big.txt

# 将扫描结果保存到文件中
dirb https://www.baidu.com -o output.txt /usr/share/wordlists/dirb/big.txt

# 列举指定后缀名目录
dirb https://www.baidu.com -X .php /usr/share/wordlists/dirb/big.txt

# 添加毫秒延迟，避免洪水攻击（单位是毫秒）
dirb https://www.baidu.com/ -z 100
```
扩展：<br />common.txt被设置为目录遍历的默认单词列表，后期可以把自己觉得好的目录名字，追加到这个字典中，进一步完善字典（路径：/usr/share/dirb/wordlists/common.txt）

<a name="FP4fr"></a>
# 二、kali自带扫描工具-dirbuster
>  DirBuster目录扫描工具支持全部的Web目录扫描方式。它既支持网页爬虫方式扫描，也支持基于字典暴力扫描，还支持纯暴力扫描。这个软件kali系统自带有，我们可以直接使用，操作也很简单。  

![1683109509530-b39c805b-c26d-4adb-84e4-2ea1f54ba74e.png](/images/1683109509530-b39c805b-c26d-4adb-84e4-2ea1f54ba74e.png)

![4ef82c6e023542719ddab478fef94e64](/images/4ef82c6e023542719ddab478fef94e64.png)![803e21b689974a809e1d20efa76eb114](/images/803e21b689974a809e1d20efa76eb114.png)

# 三、其它扫描工具--gobuster、dirsearch、nikto
后续学习

