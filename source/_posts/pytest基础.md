---
title: pytest基础
date: 2023-09-19 17:20:15
categories: #分类
- 自动化测试
tags: #标签
  - pytest

typora-root-url: ./..
---
<a name="NKjZm"></a>
# 注：函数、方法都可以看做是用例
<a name="Tvl9E"></a>
# 一、pytest相关插件
在项目中新建一个requirements.txt,输入下面的插件，然后命令行输入**pip install -r requirements.txt**即可一次性安装
```
pytest
pytest-html
pytest-xdist
pytest-ordering
pytest-rerunfailures
allure-pytest
  
```
pytest-html---生成html格式的自动化测试报告<br />pytest-xdist---测试用例分布式执行。多CPU分发<br />pytest-ordering---用于改变测试用例的执行顺序<br />pytest-rerunfailures---用例失败后重跑<br />allure-pytest-----生成美观的测试报告<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658319549293-cf5b3f71-eac9-4b8e-9ba2-fb96ce898eb8.png#averageHue=%235e5b33&clientId=u430eaf21-9cf6-4&from=paste&height=246&id=u7f1a3547&originHeight=307&originWidth=524&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24470&status=done&style=none&taskId=u81c7100e-91f1-4113-9292-d42ad54399e&title=&width=419.2)
<a name="TpHIF"></a>
# 二、运行方式
<a name="CRpZ4"></a>
## （1）主函数运行---pytest.main()
参数-s：表示 输出调试信息，包括print打印的信息，如果不加-s,print的信息无法输出
```php
注：默认执行当前目录中所有以test_开头的用例，即使不在同一个模块文件内
if __name__='__main__':
#1、不加参数 
    pytest.main()
#2、加参数
    pytest.main(['-s'])
#3、执行指定用例
    pytest.main(['文件名.py'])
#4、参数+指定用例
    pytest.main(['-vs','文件名.py'])
    #开多线程 pytest.main(['-vs','./testcase','-n=2'])，前提是安装了pytest-xdist
#5、执行指定目录下的用例,例如项目根目录下的testcase内的所有用例
    pytest.main(['-vs','./testcase'])
#6、执行
    pytest.main(['-vs'],'./目录/文件名.py::类名::用例名')
    #执行函数也一样
```
运行：1、运行根目录下的all.py，执行了项目中的所有用例，演示项目中有两个测试用例模块，共3个用例![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658315976679-80053acd-e615-475a-97c9-7290abc31b70.png#averageHue=%23837455&clientId=u430eaf21-9cf6-4&from=paste&height=522&id=uda685f0e&originHeight=653&originWidth=970&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77450&status=done&style=none&taskId=u77580468-2ee1-481c-9b9b-c4fce7a86fa&title=&width=776)
<a name="wKtlR"></a>
## （2）命令行模式
```php
1、运行所有：pytest
2、指定模块：pytest -vs 文件名.py
3、指定目录：pytest -vs ./目录名
```
<a name="Lq0uI"></a>
## (3)pytest.ini
pytest.ini是pytest单元测试框架的核心配置文件--文件中不能使用任何中文符号（也可以用，最好少用）

a、位置：一般放项目的根目录<br />b、编码：ANSI，可以使用notepad++修改编码格式<br />c、作用：改变pytest默认行为<br />d、不管是主函数还是命令行运行，都会读取这个配置文件
```
[pytest]
注释符号用分号；
;命令行参数，用空格分隔
addopts = -v -s --reruns 1 --html=report.html
; 读取测试用例的起始文件夹，多个路径用空格分隔。注意：这些目录下不能出现相同文件名，否则会报错
testpaths = ./testcase
; 配置测试搜索的模块文件名称
python_files =  test_*.py
; 配置测试搜索的模块文件名称
python_classes=Test*
python_functions=test

;标记分组参数
markers=
    smoke:冒烟用例
    usermanage:用户管理模块

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1663053901608-f62292e1-ef68-45b5-bc8e-6193d411f59e.png#averageHue=%236a6543&clientId=u97b14d32-0d0d-4&from=paste&height=298&id=u8bd68124&originHeight=298&originWidth=484&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38712&status=done&style=none&taskId=ue34c6879-2282-416f-8342-fc5fa2719a6&title=&width=484)
<a name="ULq7Q"></a>
# 二、参数详解：
**-s**:显示输出调试信息，包括print打印的信息<br />**-v**:显示理详细的信息<br />**-n**：支持多线程或者分布式运行测试用例。<br />如pytest -vs ./testcase/test.py -n 2  ------>这一行就是开了两个线程用来执行testcase/test.py内的所有用例，如果仅一个线程执行3个用例需要9秒，那么用两个线程来执行大约用6秒（即一个线程跑3次，两个线程只跑2次）<br />**--reruns num**:失败用例重跑(需安装pytest插件)
```php
#当用例执行失败，再重新多跑2次
1、main方式
  pytest.main(['-vs','./testcase','--reruns=2'])     
2、命令行
  pytest -vs ./testcase --reruns 2
```
**-x**:只要一个用例报错，测试停止<br />**--maxfail=2 **:出现两个用例失败则停止

三、用例执行顺序 <br />1、默认用例从上到下执行<br />2、改变顺序<br />第一种--使用mark标记：@pytest.mark.run(order=num)<br />第二种--使用pytest.ini<br />a、pytest.ini中设置分组<br />b、用例前加上@pytest.mark.分组名<br />c、执行pytest -v -m "smoke"<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1658322989916-1a8670ed-4605-4de2-9aee-802e55418dfe.png#averageHue=%237f6f4b&clientId=u430eaf21-9cf6-4&from=paste&height=682&id=u8114949d&originHeight=853&originWidth=1472&originalType=binary&ratio=1&rotation=0&showTitle=false&size=164304&status=done&style=none&taskId=uda2ea360-e600-423b-94c3-6ca21c6dc0f&title=&width=1177.6)
