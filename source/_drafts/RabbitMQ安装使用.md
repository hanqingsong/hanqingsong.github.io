---
title: RabbitMQ安装使用
date: 2018-05-31 15:45:32
categories:
    - RabbitMQ
tags:
    - RabbitMQ
---
## 特性
可靠性：RabbitMQ提供多种功能让你能够在性能与可靠性之间做出权衡，包括持久化、消息投递确认、发布者确认和高用性。

灵活的路由：消息在到达queues之前，通过exchanges进行路由。RabbitMQ为典型的路由逻辑內建了多种exchange类型。你能够将多种exchanges绑定在一起，也能够以插件的形式，自己实现一个exchange。

集群：在同一个本地网络中的多个RabbitMQ server能够组成集群，形成一个统一的逻辑broker。

联合：如果多个server只需要一个松散的、不可靠的连接，则可以通过RabbitMQ的联合模式实现。

高可用的queues：Queues能够在集群内多台机器之间镜像，确保在某一台机器故障时的消息安全。

多协议：RabbitMQ支持多种消息协议。核心协议是AMQP 0-9-1，另外还能够以插件的形式，支持STOMP、MQTT、AMQP1.0等。

多客户端：你能想到的任何语言几乎都有RabbitMQ的客户端。

管理界面：提供容易使用的管理界面来监控broker。

消息跟踪：RabbitMQ提供消息跟踪功能。

支持插件：支持以插件的形式，扩展功能。


## 资料
介绍https://bbs.huaweicloud.com/blogs/1a0286c8eba811e79fc57ca23e93a89f