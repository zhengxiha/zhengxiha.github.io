---
title: conftest.py文件使用
date: 2023-05-28 17:20:15
categories: #分类
- 自动化测试
tags: #标签
  - pytest

typora-root-url: ./..
---

一、conftest.py<br />1、confest.py是专门用于存放fixtrue的配置文件，名称固定<br />2、在conftest.py文件中的所有方法在调用时都不需要导包<br />3、conftest.py文件可以有多个，且多个conftest.py文件里面的多个fixtrue可以被一个用例调用<br />可分层管理，<br />二、前后置<br />1、基本的前后置
```
def setup(self)---用例前执行
def teardown(self)---用例后执行
def setup_class(self)---类前执行
def teardown_class(self)---类后执行
```
2、fixtrue装饰器-部分前后置（一般配合conftest.py使用，就是写在conftest.py文件中）
```
@pytes.fixtrue(scope='作用域'，params='数据驱动',autouse='自动执行'，
               ids='数据驱动时重命名参数名',name='给fixtrue作用的函数重命名')
               
       
-------scope参数--------
function 函数之前/后执行
class 类之前/后执行
module 模块之前/后执行 （不太常用）
package/session 回话前/后执行

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1662633157368-b10644c8-b87f-4dde-b95b-2eac1e034b60.png#averageHue=%23fcfcfb&clientId=ub232078a-ab13-4&from=paste&height=207&id=u2f653244&originHeight=207&originWidth=469&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55155&status=done&style=none&taskId=u96d2d16a-c764-466e-980f-a93b4053370&title=&width=469)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1662633420559-5fc088c6-5151-408c-b459-52741fb1bab3.png#averageHue=%23e2e0db&clientId=ub232078a-ab13-4&from=paste&height=163&id=u95bc3d4e&originHeight=163&originWidth=453&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70946&status=done&style=none&taskId=u30a5de41-20bc-422f-b553-ddecacb06db&title=&width=453)
