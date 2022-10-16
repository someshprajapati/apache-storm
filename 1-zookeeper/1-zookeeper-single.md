### System Info
```
ubuntu@nimbus:~$ uname -a
Linux nimbus 4.15.0-194-generic #205-Ubuntu SMP Fri Sep 16 19:49:27 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

ubuntu@supervisor-1:~$ uname -a
Linux nimbus 4.15.0-194-generic #205-Ubuntu SMP Fri Sep 16 19:49:27 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

ubuntu@supervisor-2:~$ uname -a
Linux nimbus 4.15.0-194-generic #205-Ubuntu SMP Fri Sep 16 19:49:27 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

### Install basic Packages
`sudo apt-get update`

`sudo apt-get -y install wget ca-certificates zip net-tools vim nano tar netcat`

### Java Open JDK 8
`sudo apt-get -y install openjdk-8-jdk`

`java -version`

> Results:
```
ubuntu@nimbus:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)

ubuntu@supervisor-1:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)

ubuntu@supervisor-2:~$ java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-8u342-b07-0ubuntu1~18.04-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)
```

### Storm need python version > 2.6 so, install python packages
`sudo apt install python`

> Results:
```
ubuntu@nimbus:~/apache-storm$ python --version
Python 2.7.17
```

### Disable RAM Swap - can set to 0 on certain Linux distro
`sudo sysctl vm.swappiness=1`

`echo 'vm.swappiness=1' | sudo tee --append /etc/sysctl.conf`

### Add hosts entries (mocking DNS) - put relevant IPs here
```
echo "192.168.1.11 zookeeper1
192.168.1.12 zookeeper2
192.168.1.13 zookeeper3" | sudo tee --append /etc/hosts
```

### Download Zookeeper (3.4.11)
```
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz

tar -xvzf zookeeper-3.4.11.tar.gz
rm zookeeper-3.4.11.tar.gz
mv zookeeper-3.4.11 zookeeper
cd zookeeper/
```

### Edit `bashrc` and update the Zookeeper path
`vi ~/.bashrc`

```
export ZOOKEEPER_HOME=/home/ubuntu/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```

> Results:
```
ubuntu@nimbus:~$ vi ~/.bashrc

ubuntu@nimbus:~$ exec bash

ubuntu@nimbus:~$ head -3 ~/.bashrc
export ZOOKEEPER_HOME=/home/ubuntu/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```

### Verify if the PATH is correctly setup

```
ubuntu@nimbus:~$ echo $ZOOKEEPER_HOME
/home/ubuntu/zookeeper

ubuntu@nimbus:~$ ls -l $ZOOKEEPER_HOME/bin
total 36
-rwxr-xr-x 1 ubuntu ubuntu  232 Nov  1  2017 README.txt
-rwxr-xr-x 1 ubuntu ubuntu 1937 Nov  1  2017 zkCleanup.sh
-rwxr-xr-x 1 ubuntu ubuntu 1056 Nov  1  2017 zkCli.cmd
-rwxr-xr-x 1 ubuntu ubuntu 1534 Nov  1  2017 zkCli.sh
-rwxr-xr-x 1 ubuntu ubuntu 1628 Nov  1  2017 zkEnv.cmd
-rwxr-xr-x 1 ubuntu ubuntu 2696 Nov  1  2017 zkEnv.sh
-rwxr-xr-x 1 ubuntu ubuntu 1089 Nov  1  2017 zkServer.cmd
-rwxr-xr-x 1 ubuntu ubuntu 6773 Nov  1  2017 zkServer.sh
```

### Create data directory for zookeeper in all servers
`sudo mkdir -p /data/zookeeper`

`sudo chown -R ubuntu:ubuntu /data/`


### Zookeeper configurations (Copy file from `zoo.cfg`)
`vim /home/ubuntu/zookeeper/conf/zoo.cfg`

> Results:
```
ubuntu@nimbus:~/zookeeper/conf$ cat zoo.cfg
# the location to store the in-memory database snapshots and, unless specified otherwise, the transaction log of updates to the database.
dataDir=/data/zookeeper
# the port at which the clients will connect
clientPort=2181
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=0
# the basic time unit in milliseconds used by ZooKeeper. It is used to do heartbeats and the minimum session timeout will be twice the tickTime.
tickTime=2000
# The number of ticks that the initial synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# zoo servers
# these hostnames such as `zookeeper-1` come from the /etc/hosts file
server.1=zookeeper1:2888:3888


ubuntu@nimbus:~/zookeeper$ tail -1 conf/zoo.cfg
server.1=zookeeper1:2888:3888
```

### Check if ZooKeeper service is running or not
`jps`

> Results:
```
ubuntu@nimbus:~/zookeeper/conf$ jps
19061 Jps
```

### Start Zookeeper
`zkServer.sh start`

> Results:
```
ubuntu@nimbus:~/zookeeper$ zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

### Check if Zookeeper is running or not (Check the `Mode`)
`jps`

`zkServer.sh status`

> Results:
```
ubuntu@nimbus:~/zookeeper$ jps
19195 QuorumPeerMain
19214 Jps

ubuntu@nimbus:~/zookeeper$ zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /home/ubuntu/zookeeper/bin/../conf/zoo.cfg
Mode: standalone
```

### Demonstrate the use of a 4 letter word
`echo "ruok" | nc localhost 2181 ; echo`

> Results:
```
imok
```

### Verify it's connected
`nc -vz localhost 2181`

> Results:
```
ubuntu@nimbus:~/zookeeper$ nc -vz localhost 2181
Connection to localhost 2181 port [tcp/*] succeeded!
```

### Verify it's started
`nc -vz localhost 2181`
`echo "ruok" | nc localhost 2181 ; echo`

### Check the logs
`ubuntu@nimbus:~/zookeeper$ cat zookeeper.out`


Next: [Zookeeper Quorum](2-zookeeper-quorum.md)
