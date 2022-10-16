# ZooKeeper setup on Storm2 and Storm3 servers
Follow the same steps on Storm2 and Storm3 servers which we followed on Storm1 server previously till Step[`Create data dictionary for zookeeper in all servers`]: [zookeeper-standalone](1-zookeeper-standalone.md)

### Zookeeper configurations (Copy file from `zoo.cfg`)
`vim /home/ubuntu/zookeeper/conf/zoo.cfg`

> Results:
```
ubuntu@nimbus:~/zookeeper$ tail -4 conf/zoo.cfg
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888

ubuntu@supervisor-1:~/zookeeper$ tail -4 conf/zoo.cfg
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888

ubuntu@supervisor-2:~/zookeeper$ tail -4 conf/zoo.cfg
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```


### Create data directory for zookeeper in all servers
`sudo mkdir -p /data/zookeeper`

`sudo chown -R ubuntu:ubuntu /data/`

### Declare the server's identity
`echo "1" > /data/zookeeper/myid`

> Results:
```
ubuntu@nimbus:~/zookeeper$ echo "1" > /data/zookeeper/myid
ubuntu@nimbus:~/zookeeper$ cat /data/zookeeper/myid
1

ubuntu@supervisor-1:~/zookeeper$ echo "2" > /data/zookeeper/myid
ubuntu@supervisor-1:~/zookeeper$ cat /data/zookeeper/myid
2

ubuntu@supervisor-2:~/zookeeper$ echo "3" > /data/zookeeper/myid
ubuntu@supervisor-2:~/zookeeper$ cat /data/zookeeper/myid
3
```


### Start the zookeeper service
`zkServer.sh start`

> Results:
```
ubuntu@nimbus:~/zookeeper$ zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED


ubuntu@supervisor-1:~/zookeeper$ zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED


ubuntu@supervisor-2:~/zookeeper$ zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```


### Check the zookeeper service status
`zkServer.sh status`

> Results:
```
ubuntu@nimbus:~/zookeeper$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Mode: leader


ubuntu@supervisor-1:~/zookeeper$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Mode: follower


ubuntu@supervisor-2:~/zookeeper$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Mode: follower
```


### Verify if the Quorum is working, check on any server
`echo "ruok" | nc zookeeper1 2181 ; echo`

`echo "stat" | nc zookeeper1 2181 ; echo`

> Results:
```
ubuntu@nimbus:~/zookeeper$ echo "ruok" | nc zookeeper1 2181 ; echo
imok

ubuntu@supervisor-1:~/zookeeper$ echo "ruok" | nc zookeeper1 2181 ; echo
imok

ubuntu@supervisor-2:~/zookeeper$ echo "ruok" | nc zookeeper1 2181 ; echo
imok
```

> Stats Results:
```
ubuntu@nimbus:~/zookeeper$ echo "stat" | nc zookeeper1 2181 ; echo
Zookeeper version: 3.4.11-37e277162d567b55a07d1755f0b31c32e93c01a0, built on 11/01/2017 18:06 GMT
Clients:
 /127.0.0.1:40490[1](queued=0,recved=254,sent=254)
 /127.0.0.1:40502[1](queued=0,recved=494,sent=494)
 /127.0.0.1:40496[1](queued=0,recved=60,sent=60)
 /127.0.0.1:40516[1](queued=0,recved=67,sent=67)
 /127.0.0.1:40512[1](queued=0,recved=54,sent=54)
 /192.168.1.11:50056[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/17
Received: 938
Sent: 936
Connections: 8
Outstanding: 0
Zxid: 0x100000055
Mode: leader
Node count: 18


ubuntu@supervisor-1:~/zookeeper$ echo "stat" | nc zookeeper1 2181 ; echo
Zookeeper version: 3.4.11-37e277162d567b55a07d1755f0b31c32e93c01a0, built on 11/01/2017 18:06 GMT
Clients:
 /127.0.0.1:40490[1](queued=0,recved=254,sent=254)
 /127.0.0.1:40502[1](queued=0,recved=494,sent=494)
 /127.0.0.1:40496[1](queued=0,recved=60,sent=60)
 /127.0.0.1:40516[1](queued=0,recved=67,sent=67)
 /192.168.1.13:44072[0](queued=0,recved=1,sent=0)
 /192.168.1.12:49790[0](queued=0,recved=1,sent=0)
 /127.0.0.1:40512[1](queued=0,recved=54,sent=54)
 null[0](queued=0,recved=1,sent=1)

Latency min/avg/max: 0/0/17
Received: 939
Sent: 937
Connections: 7
Outstanding: 0
Zxid: 0x100000055
Mode: leader
Node count: 18


ubuntu@supervisor-2:~/zookeeper$ echo "stat" | nc zookeeper1 2181 ; echo
Zookeeper version: 3.4.11-37e277162d567b55a07d1755f0b31c32e93c01a0, built on 11/01/2017 18:06 GMT
Clients:
 /127.0.0.1:40490[1](queued=0,recved=254,sent=254)
 /127.0.0.1:40502[1](queued=0,recved=494,sent=494)
 /127.0.0.1:40496[1](queued=0,recved=60,sent=60)
 /127.0.0.1:40516[1](queued=0,recved=67,sent=67)
 /192.168.1.13:44072[0](queued=0,recved=1,sent=0)
 /192.168.1.12:49790[0](queued=0,recved=1,sent=0)
 /127.0.0.1:40512[1](queued=0,recved=54,sent=54)
 /192.168.1.11:50056[0](queued=0,recved=1,sent=1)

Latency min/avg/max: 0/0/17
Received: 939
Sent: 937
Connections: 7
Outstanding: 0
Zxid: 0x100000055
Mode: leader
Node count: 18
```

### Observe the logs - need to do this on every machine
```
ubuntu@nimbus:~/zookeeper$ cat zookeeper.out | head -100
```
