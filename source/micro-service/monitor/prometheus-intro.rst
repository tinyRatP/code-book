Prometheus 简单入门教程
#######################

随着容器化技术的发展，微服务成为了后端开发的现代化设计方式。

大量微服务部署到服务器之上后，如何监控各个微服务的关键指标则成为了一项难题。各个监控框架应运而生，但毫无疑问，Prometheus 是其中最出色的一个。

简单介绍
========

.. figure:: /_static/prometheus-intro/prometheus-release-roadmaps.png
   :alt: Prometheus 发展历程
   :align: center

`Prometheus <https://prometheus.io/>`_ 是一套开源的用 Golang 实现的系统监控告警框架，由工作在 SoundCloud 的前 Google 员工在 2012 年开始进行开发，并于 2015 年在开源社区进行正式发布。

2018 年，Prometheus 正式从 `CNCF <https://www.cncf.io/>`_ （Cloud Native Computing Foundation）毕业。这意味着 Prometheus 已经具备了一定的成熟度和稳定性，可以放心地作为企业级应用。

无论是常规的服务器数据（CPU 负荷，磁盘使用量等等），还是难以监控的业务数据（接口请求响应时间，MySQL 连接数，用户注册量等等），Prometheus 都能轻松应对。

与同类监控系统相比，Prometheus 具备许多特性：

- 多维度的数据模型支持
- 灵活的查询语言 PromQL
- 通过服务发现动态获取监控主机
- 支持多种多样的图表和界面展示，例如 Grafana
- 基于 HTTP Pull 方式采集时序数据
- 不依赖分布式存储，单个服务器节点完全自主
- ...

.. note:: 时序数据是在不同时间上收集得到的数据，主要以时间维度，描述数据的变化状态。

服务架构
=========

.. figure:: /_static/prometheus-intro/prometheus-architecture.png
    :alt: Prometheus Architecture
    :align: center

    Prometheus 系统架构

Prometheus 架构中包含了多个组件，其中有些必需的，有些则是可选的。

Exporter
++++++++

Exporter 是 Prometheus 中数据采集组件的总称。它主要负责从主机或程序里搜集数据，并将其转义为 Prometheus 支持的格式。

Prometheus Server
+++++++++++++++++

Prometheus Server 又分为三个更小的组件：

- Retrieval：负责去收集各个 Target 暴露的监控数据
- TSDB：负责将搜集的监控数据转义为时序数据，并存在磁盘或 InfluxDB 中
- HTTP Server：提供 HTTP API 接口，允许第三方组件使用 PromQL 查询数据


Alertmanager
++++++++++++

接收来自 Prometheus Server 产生的告警信息，并根据规则再次封装，最后发送告警给相应终端（Email，Dingding，Wechat 等等）。

Prometheus web UI
+++++++++++++++++

使用 PromQL 语句查询存储起来的时序数据，并在 Web 以图表的形式展示。

Prometheus 的工作流程
=====================

1. Exporter 采集需要的数据
2. Prometheus Server 周期性从 Exporter 拉取最新的数据，并存储在 TSDB 中
3. Prometheus Server 根据已经定义好的告警规则，将 ``AlertEvent`` 发送给 Alertmanager
4. Alertmanager 根据规则接收 AlertEvent 进行处理，然后发送警报
5. web UI 可视化界面展示数据状态

四种数据维度（Metrics）
=======================

Prometheus 提供了四种数据模型来收集来作为所有数据的基础：

- Counter：只增数据模型，数据变化只能在现有的基础上进行增加，无法减少，默认值为 0
- Gauge：常规数据模型，数据变化可以任意设置，增加或减少
- Histogram 和 Summary：都用于反映数据的分布情况，但又有些许差别

Histogram 和 Summary
++++++++++++++++++++

hello

如何编写 Exporter
------------------

Prometheus 官方提供了许多已经写好的 `Node Exporter <https://prometheus.io/docs/instrumenting/exporters/>`_ 代码，包括有数据库，硬件，消息中间件，日志等等。

如果需要根据自己的业务需求来采集数据，可以通过 Prometheus 提供的 `Client Libraries <https://prometheus.io/docs/instrumenting/clientlibs/>`_ 来编写自己的采集逻辑。

首先，安装 Prometheus Client 的 SDK。

.. code-block:: shell

   $ go get -u github.com/prometheus/client_golang


`完整代码 <https://gist.github.com/tinyRatP/04cc44e2c95a4354bc449db87cd1accb#file-go_kit_prometheus-go>`_

参考文章
=========

- `Promethes Overview <https://prometheus.io/>`_, Prometheus Doc
- `Prometheus 入门与实践 <https://developer.ibm.com/zh/depmodels/cloud/articles/cl-lo-prometheus-getting-started-and-practice/>`_, IBM Developer
- `Prometheus 介紹與基礎入門 <https://k2r2bai.com/2018/06/10/cncf/prometheus/>`_, KaiRen's Blog
