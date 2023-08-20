# Server

```
./zkServer.sh start
./zkServer.sh stop
./zkServer.sh status
./zkServer.sh restart
```

# Client

```
./zkCli.sh -server 192.168.10.10:2181
quit

# 显示指定目录下的节点
ls /
ls /dubbo
# 查询节点的详细信息
ls -s /dubbo

# 创建持久化节点
create /app data1

# 创建临时节点,本次会话有效
create -e /app data1 

# 创建顺序节点(持久化节点)
create -s /app data1 

# 创建临时持久化节点
create -3s /app data1 

# 获取节点值
get /app
# 设置节点值
set /app data2
# 删除单个节点
delete /app
# 删除带有子节点的节点
deleteall /app

help
```

# 集群角色

- Leader领导者
  - 1、处理事务请求(**增删改**)
  - 2、集群内部给服务器的调度着
- Follower跟随者
  - 1、处理客户端非事务请求(**查**)，转发事务请求给Leader服务器
  - 2、**参与**Leader选举投票
- Observer观察者
  - 1、处理客户端非事务请求(**查**)，转发事务请求给Leader服务器
  - 2、**不参与**Leader选举投票

# Leader选举

- Serverid：服务器ID
  - 比如有三台服务器，编号分别是1，2，3。编号越大在选举算法中的权重越大。
- Zxid：数据ID
  - 服务器中存放的最大数据ID，值越大说明数据更新越频繁，在选举算法中的权重越大。
- 在Leader选举的过程中，如果某台ZooKeeper获得了超过半数的选票，则此ZooKeeper就可以称为Leader。

# 集群搭建

1、创建data数据目录，并将zoo_sample.cfg文件改名为zoo.cfg

```
mkdir /opt/zookeeper-1/data
mkdir /opt/zookeeper-2/data
mkdir /opt/zookeeper-3/data

cp /opt/zookeeper-1/conf/zoo_sample.cfg /opt/zookeeper-1/conf/zoo.cfg
cp /opt/zookeeper-2/conf/zoo_sample.cfg /opt/zookeeper-2/conf/zoo.cfg
cp /opt/zookeeper-3/conf/zoo_sample.cfg /opt/zookeeper-3/conf/zoo.cfg
```

2、配置每一个zookeeper的dataDir和clientPort

```
vim /opt/zookeeper-1/conf/zoo.cfg
dataDir=/opt/zookeeper-1/data
clientPort=2181

vim /opt/zookeeper-2/conf/zoo.cfg
dataDir=/opt/zookeeper-2/data
clientPort=2182

vim /opt/zookeeper-3/conf/zoo.cfg
dataDir=/opt/zookeeper-3/data
clientPort=2183
```

3、在每个zookeeper的data目录下创建一个myid的文件，内容分别为1、2、3。这个文件就是记录每个服务器的ID。

```
echo 1 > /opt/zookeeper-1/data/myid
echo 2 > /opt/zookeeper-2/data/myid
echo 3 > /opt/zookeeper-3/data/myid
```

4、在每一个zookeeper的zoo.cfg文件中配置集群服务器IP列表

```
vim /opt/zookeeper-1/conf/zoo.cfg
vim /opt/zookeeper-2/conf/zoo.cfg
vim /opt/zookeeper-3/conf/zoo.cfg

# server.服务器ID=服务ID地址:服务器之间通信端口:服务器之间投票选举端口
server.1=192.168.10.10:2881:3881
server.2=192.168.10.10:2882:3882
server.3=192.168.10.10:2883:3883
```

5、启动服务器

```
/opt/zookeeper-1/bin/zkServer.sh start
/opt/zookeeper-2/bin/zkServer.sh start
/opt/zookeeper-3/bin/zkServer.sh start
```

