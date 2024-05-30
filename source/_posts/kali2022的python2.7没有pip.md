---
title: kali2022的python2.7没有pip
date: 2023-05-04 17:20:15
categories: #分类
- 渗透测试
tags: #标签
  - Kali


---



1、进入[pip](https://pypi.org/project/pip/#history)下载对应版本，或直接复制下载版本的链接<br />当前kali2022内的python3.10的pip版本是22.3,我下载了这个版本后用python2.7安装，没有用，所以改20.0.1![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682694772209-153c0f0b-86ce-4c29-9b97-3abd0e2cbae0.png#averageHue=%23242833&clientId=u5b1c6edb-15e4-4&from=paste&height=79&id=uc5c4fe09&originHeight=79&originWidth=569&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12259&status=done&style=none&taskId=u323b4496-ed22-4649-8d1c-ae4794a6d36&title=&width=569)

```java

// 切换到python2.7的pip安装目录  
cd /usr/lib/python2.7/dist-packages/
// 下载pip 
wget https://files.pythonhosted.org/packages/28/af/2c76c8aa46ccdf7578b83d97a11a2d1858794d4be4a1610ade0d30182e8b/pip-20.0.1.tar.gz
// 解压
tar -xvf pip-20.0.1.tar.gz
// 进入解压目录
cd pip-20.0.1
// 执行如下
python2.7 setup.py build
python2.7 setup.py install
python2.7 -m pip install --upgrade setuptools

    
```

验证成功<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1682695194063-55c4018c-a67e-4403-9c04-39bc0d1fb4ea.png#averageHue=%23292f43&clientId=u5b1c6edb-15e4-4&from=paste&height=99&id=ud86ecb42&originHeight=99&originWidth=634&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19590&status=done&style=none&taskId=u3360d087-3ddb-404e-9204-9a65b3d41b2&title=&width=634)
