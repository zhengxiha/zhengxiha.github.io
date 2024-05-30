---
title: 性能监控工具：prometheus+grafana
date: 2023-07-17 17:20:15
categories: #分类
- 性能测试
tags: #标签
  - jmeter


---

<a name="x4MKx"></a>
# 一、exporter、prometheus和grafana
[Exporter是什么 · Prometheus中文技术文档](https://www.prometheus.wang/exporter/what-is-prometheus-exporter.html)
> 广义上讲所有可以向Prometheus**提供监控样本数据**的程序都可以被称为一个Exporter。而Exporter的一个实例称为target，如下所示，Prometheus通过轮询的方式定期从这些target中获取样本数据:

![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1689237114049-24224af4-e47c-4b94-a973-0b0f8ea7806d.png#averageHue=%23faf9f8&clientId=uede48196-0018-4&from=paste&id=u883504ef&originHeight=254&originWidth=1018&originalType=url&ratio=1&rotation=0&showTitle=false&size=21894&status=done&style=none&taskId=u12aed51a-b12e-41d3-8795-0a337fed4d6&title=)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/25987745/1689237225362-f7e1cfd4-61d8-4515-abb5-3971a85f6278.jpeg)<br />总的来说，就是每台需要被监控的服务器都要先安装exporter，exporter会一直监听服务器的各个服务，然后将这些数据(内存、cpu、磁盘等)发送给prometheus，那么prometheus就可以实时监控这些服务器的状态；<br />而grafana连接prometheus，可以把prometheus的数据同步过来，因为grafana有许多模块，所以能够更加直观地展示这些指标
<a name="kZtDR"></a>
# 二、安装
准备工作：

- 本次搭建为方便仅用一台机器，所以服务端和客户端都是一个IP：192.168.131.129  	ubuntu-22.04.2  
<a name="NQj36"></a>
## 被监测端（客户端）

- 在所有需要监测的服务器上安装node_exporter
<a name="DuLva"></a>
### 安装exporter
```shell
#下载
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
#解压
tar -zxvf node_exporter-1.3.1.linux-amd64.tar.gz
#重命名
mv node_exporter-1.3.1.linux-amd64 node_exporter

```
<a name="SfKTr"></a>
### 启动node_exporter
```shell
./node_exporter 
```
启动后，默认监听端口为9100，在浏览器输入，192.168.152.101:9100可以查看访问 Client 的监控指标；进入metrics可以看到相关信息<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1689238122187-6a7d92dc-ce8c-4cdc-a5c5-107672291b2e.png#averageHue=%23efedeb&clientId=uede48196-0018-4&from=paste&height=675&id=ua9e0f0dd&originHeight=675&originWidth=901&originalType=binary&ratio=1&rotation=0&showTitle=false&size=330305&status=done&style=none&taskId=ua23781b4-d91b-43f2-b661-1d2de7f8dff&title=&width=901)
<a name="ikhYS"></a>
## 查看端（服务器）
<a name="Hj8VV"></a>
### 下载
服务端下载prometheus：[https://github.com/prometheus/prometheus/releases](https://github.com/prometheus/prometheus/releases)<br />找到自己的linux版本，大多数是linux-amd64<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/25987745/1689238500694-fcf5fc70-5d14-41c4-a86b-9051a7955612.png#averageHue=%23fefefd&clientId=uede48196-0018-4&from=paste&height=465&id=u03e7209b&originHeight=465&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76111&status=done&style=none&taskId=u4fb136b3-212c-467c-a68f-a33822ad777&title=&width=507)<br />下载完只关注两个文件

- 启动文件：prometheus
- 配置文件：prometheus.yml
<a name="dSeEM"></a>
### 服务端与客户端连接
```shell
cat prometheus.yml

```
```shell
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['localhost:9090']
# ------在这里 新增如下内容，job_name可以随便命名，targets是刚刚客户端的IP和端口
  - job_name: 'server'
    static_configs:
      - targets:  ['localhost:9100']
```
<a name="ajvlD"></a>
### 启动
```shell
./prometheus --config.file=prometheus.yml

```
此时登陆9090端口可以测试看看，进入status>>targets
<a name="pPnQ2"></a>
## 第三方界面安装--grafana
```shell
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/enterprise/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana-enterprise
# 启动
sudo systemctl start grafana-server.service

```
s访问Grafana，登录http://192.168.152.103:3000/，默认账号密码：admin，admin<br />添加数据 add data source<br />。。。。。<br />后面添加数据源和模板即可，懒得写了，网上找
<a name="R37vk"></a>
## 后台启动
服务端：
```shell
#不保存日志
nohup ./prometheus --config.file=prometheus.yml >/dev/null 2>&1 &
#保存日志到/var/log/prometheus.log
nohup ./prometheus --config.file=prometheus.yml >/var/log/prometheus.log 2>&1 &

```
客户端：
```shell
#不保存日志
nohup ./node_exporter >/dev/null 2>&1 &
#保存日志到/var/log/node_exporter.log
nohup ./node_exporter >/var/log/node_exporter.log 2>&1 &

```
<a name="qYleR"></a>
# 问题
1、：如果出现fail开头的问题，直接重新安装一个更高grafana版本即可

参考：[普罗米修斯Prometheus+Grafana，监控搭建与界面基础配置_linux_the丶only-华为云开发者联盟](https://huaweicloud.csdn.net/6356391fd3efff3090b5b217.html?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~activity-2-125845193-blog-114697770.235^v38^pc_relevant_anti_t3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~activity-2-125845193-blog-114697770.235^v38^pc_relevant_anti_t3&utm_relevant_index=5)
