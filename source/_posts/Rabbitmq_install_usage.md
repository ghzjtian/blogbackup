---
title: RabbitMQ 安装及使用
tags: [data]
categories: [data]
date: 2019-12-26 17:46:19
---


> RabbitMQ 的使用方法

<!-- more -->



## [1.参考](#references)
## [2.Mac 安装过程](#install_guide)
## [3.使用](#usage)
## [4.命令行创建 exchange, queue, binding](#declare_exchange)


***
***
***

## 1.参考<a name="references"/>
* 0.[RabbitMQ](https://www.rabbitmq.com/)
* 1.[Github rabbitmq/rabbitmq-tutorials](https://github.com/rabbitmq/rabbitmq-tutorials)
* 2.[rabbitmqctl(8)](https://www.rabbitmq.com/rabbitmqctl.8.html#set_user_tags)
* 3.[How do I create or add a user to rabbitmq?](https://stackoverflow.com/a/40436752/5237440)

## 2.Mac 安装过程<a name="install_guide"/>
* 1.更新 brew

```
brew update
```

* 2.安装 RabbitMQ server

```
brew install rabbitmq
// 可能要等待很久，建议 brew 换成国内的源.
// 如果 vpn 不稳定，建议不要使用 vpn.
```

* 3.添加 `rabbitmq/sbin` 路径到 PATH 中

```
$cd ~
$vi .bash_profile
// 添加下面的一行到 .bash_profile 中.
export PATH=$PATH:/usr/local/opt/rabbitmq/sbin
$ echo $PATH
/usr/local/opt/rabbitmq/sbin


```

* 4.安装完成后，如果需要查看管理台，需要下面命令去开启

```
$rabbitmq-plugins enable rabbitmq_management
```

## 3.使用<a name="usage"/>

* 1.Server 端开启， terminal 输入 : `rabbitmq-server`
* 2.客户端开启: 运行程序.


## 4.命令行创建 exchange, queue, binding<a name="declare_exchange"/>

> 1.有 命令行 和 rest api 两种方法.我们这里讲解 命令行的用法
> 2.如果是 Windows 系统的话，需要安装 python , 而且要在命令的前面添加 `python rabbitmqadmin ...` 这样， rabbitmqadmin 从 `localhost:15672` 上下载，然后放置到当前运行的目录下面.

#### 1.参考
* 1.[How to create an exchange using rabbitmqctl](https://stackoverflow.com/questions/45952026/how-to-create-an-exchange-using-rabbitmqctl)
* 2.[Management Command Line Tool](https://www.rabbitmq.com/management-cli.html)
* 3.[Management Command Line Tool](https://www.rabbitmq.com/management-cli.html)


#### 2.主要的命令有

* 0.帮助

```
$$:RPCClient glb_gz$ rabbitmqadmin --help
$$:RPCClient glb_gz$ rabbitmqadmin help subcommands
```

* 1.定义及列出所有的 exchanges

```
$$:RPCClient glb_gz$ rabbitmqadmin declare exchange name=custom-exchange type=fanout
exchange declared
$$:RPCClient glb_gz$ rabbitmqadmin list exchanges
+--------------------+---------+
|        name        |  type   |
+--------------------+---------+
|                    | direct  |
| amq.direct         | direct  |
| amq.fanout         | fanout  |
| amq.headers        | headers |
| amq.match          | headers |
| amq.rabbitmq.trace | topic   |
| amq.topic          | topic   |
| custom-exchange    | fanout  |
| my-new-exchange    | fanout  |
| test               | fanout  |
+--------------------+---------+
```

* 2.定义及列出所有的 queues

```
$$:RPCClient glb_gz$ rabbitmqadmin declare queue name=custom-queue durable=false
queue declared
$$:RPCClient glb_gz$ rabbitmqadmin list queues
+--------------+----------+
|     name     | messages |
+--------------+----------+
| custom-queue | 0        |
| my-new-queue | 0        |
| task_queue   | 0        |
| test         | 0        |
+--------------+----------+
```

* 3.绑定 exchange 与 binding ,并且显示

```
$$:~ glb_gz$ rabbitmqadmin declare binding source=custom-exchange destination=custom-queue
binding declared
$$:~ glb_gz$ rabbitmqadmin list bindings
+-----------------+--------------+--------------+
|     source      | destination  | routing_key  |
+-----------------+--------------+--------------+
|                 | custom-queue | custom-queue |
|                 | my-new-queue | my-new-queue |
|                 | task_queue   | task_queue   |
|                 | test         | test         |
| custom-exchange | custom-queue |              |
+-----------------+--------------+--------------+
```


* 4.发布与接收 message

```
$$:~ glb_gz$ rabbitmqadmin publish exchange=custom-exchange routing_key="" payload="test 123"
Message published
$$:~ glb_gz$ rabbitmqadmin get queue=custom-queue ackmode=ack_requeue_false
+--------------+----------+---------------+----------+---------------+------------------+------------+-------------+
| routing_key  | exchange | message_count | payload  | payload_bytes | payload_encoding | properties | redelivered |
+--------------+----------+---------------+----------+---------------+------------------+------------+-------------+
| custom-queue |          | 2             | test 123 | 8             | string           |            | False       |
+--------------+----------+---------------+----------+---------------+------------------+------------+-------------+

```

* 5.显示概况

```
$$:~ glb_gz$ rabbitmqadmin show overview
+------------------+----------------------------+-----------------------+----------------------+
| rabbitmq_version |        cluster_name        | queue_totals.messages | object_totals.queues |
+------------------+----------------------------+-----------------------+----------------------+
| 3.8.2            | rabbit@$$ | 2                     | 4                    |
+------------------+----------------------------+-----------------------+----------------------+
```

#### 3.连接远程 rabbitmq server 去完成上面的命令

> 因为默认的 guest 用户不能远程访问，所以需要创建一个新的用户去完成这个操作

* 0.创建用户

```
// 添加用户
rabbitmqctl add_user glb_user xxx
// 设置权限
rabbitmqctl set_permissions -p / glb_user ".*" ".*" ".*"
// 设置标签
rabbitmqctl set_user_tags tian administrator
// 查看
rabbitmqctl list_users
```

* 1.运行 `rabbitmqadmin --help` 可以看到远程连接的命令

```
$$ rabbitmqadmin --help
Usage
=====
  rabbitmqadmin [options] subcommand

Options
=======
--help, -h              show this help message and exit
--config=CONFIG, -c CONFIG
                        configuration file [default: ~/.rabbitmqadmin.conf]
--node=NODE, -N NODE    node described in the configuration file [default:
                        'default' only if configuration file is specified]
--host=HOST, -H HOST    connect to host HOST [default: localhost]
--port=PORT, -P PORT    connect to port PORT [default: 15672]
--path-prefix=PATH_PREFIX
                        use specific URI path prefix for the RabbitMQ HTTP
                        API. /api and operation path will be appended to it.
                        (default: blank string) [default: ]
--vhost=VHOST, -V VHOST
                        connect to vhost VHOST [default: all vhosts for list,
                        '/' for declare]
--username=USERNAME, -u USERNAME
                        connect using username USERNAME [default: guest]
--password=PASSWORD, -p PASSWORD
```

* 2.例子 

```
// 需要远程计算机 192.168.1.2 先关闭防火墙 !!!
rabbitmqadmin -H 192.168.1.2 -u glb_user -p xxx list vhosts

// 发布消息
rabbitmqadmin -H 192.168.1.2 -u glb_user -p xxx publish exchange=log_direct routing_key="rule_engine" payload="log rule engine, from mac 1"
// 取得消息
rabbitmqadmin -H 192.168.1.2 -u glb_user -p xxx get queue=log_rule_engine_queue ackmode=ack_requeue_false
// No message
rabbitmqadmin -H 192.168.1.2 -u glb_user -p xxx get queue=log_message_parser_queue ackmode=ack_requeue_false

```