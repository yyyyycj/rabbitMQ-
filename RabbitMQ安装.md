# RabbitMQ安装

## 安装前操作

*安装前确保编译环境没有问题*

```
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel 
```

## 1、安装Erlang

*由于RabbitMQ是基于**Erlang（面向高并发的语言）**语言开发，所以在安装RabbitMQ之前，需要先安装Erlang。在本教程中我们将安装最新版本的Erlang到服务器中。 Erlang在默认的YUM存储库中不可用，因此您将需要安装EPEL存储库。 运行以下命令相同。*

```
yum -y install epel-release

yum -y update
```

安装Erlang

```
yum -y install erlang socat
```

查看版本

```
[root@VM-4-11-centos rabbitmq]# erl -v
```

输出如下

```
Erlang/OTP 24 [erts-12.0] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1]

Eshell V12.0  (abort with ^G)
1> 
```

## 2、下载RabbitMQ

*下载RabbitMQ需要和Erlang的版本相匹配，可以去官网选择你需要的版本*

*https://www.rabbitmq.com/install-rpm.html*

*创建文件夹，方便统一管理*

```
mkdir -p /usr/local/rabbitmq
```

*到rabbitmq目录下*

```
cd /usr/local/rabbitmq
```

*下载完成之后放在rabbitmq目录下*

## 3、安装RabbitMQ

*通过运行导入GPG密钥*

```
rpm –import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
```

*安装rpm包*

```
rpm -Uvh rabbitmq-server-3.8.16-1.el7.noarch.rpm
```

## 4、运行RabbitMQ

*到sbin目录下*

```
/usr/rabbitmq/rabbitmq_server-3.7.13/sbin
```

*运行RabbitMQ*

```
systemctl start rabbitmq-server
```

*开机自启动*

```
systemctl enable rabbitmq-server
```

*检查状态*

```
systemctl status rabbitmq-server
```

## 5. 访问Web控制台

###  启动web控制台

启动RabbitMQ Web管理控制台，方法是运行：

```
rabbitmq-plugins enable rabbitmq_management
```

通过运行以下命令，将RabbitMQ文件的所有权提供给RabbitMQ用户：

```
chown -R rabbitmq:rabbitmq /var/lib/rabbitmq/
```

### 创建用户

现在，您将需要为RabbitMQ Web管理控制台创建管理用户。 运行以下命令相同。

```
rabbitmqctl add_user admin Password
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p / admin “.*” “.*” “.*”
```

*admin为你的用户名*

*将Password更改为你的密码*

### 防火墙开端口

*开启端口*

```
[root@localhost ~]# firewall-cmd --zone=public --add-port=15672/tcp --permanent
success #这个是结果
```

命令含义：
–zone #作用域
–add-port=15672/tcp #添加端口，格式为：端口/通讯协议
–permanent #永久生效，没有此参数重启后失效

*重启防火墙*

```
　firewall-cmd --reload
```

*查看已经开放的端口*

```
[root@localhost ~]# firewall-cmd --list-port
15672/tcp #这个是结果
```

### 访问管理面板

*访问RabbitMQ的管理面板*

```
http://Your_Server_IP:15672/
```

