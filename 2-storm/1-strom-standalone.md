# Standalone Storm Installation and Configurations

### Add hosts entries (mocking DNS) - put relevant IPs here
```
echo "192.168.1.11 nimbus
192.168.1.12 supervisor-1
192.168.1.13 supervisor-2" | sudo tee --append /etc/hosts
```

### Download Apache Storm (1.1.1)
```
wget https://archive.apache.org/dist/storm/apache-storm-1.1.1/apache-storm-1.1.1.tar.gz

tar -xvzf apache-storm-1.1.1.tar.gz
rm apache-storm-1.1.1.tar.gz
mv apache-storm-1.1.1 apache-storm
cd apache-storm/
```

### Edit `bashrc` and update the Zookeeper path
`vi ~/.bashrc`

```
export ZOOKEEPER_HOME=/home/ubuntu/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
export STORM_HOME=/home/ubuntu/apache-storm
export PATH=$PATH:$STORM_HOME/bin
```

> Results:
```
ubuntu@nimbus:~$ vi ~/.bashrc

ubuntu@nimbus:~$ exec bash

ubuntu@nimbus:~$ head -4 ~/.bashrc
export ZOOKEEPER_HOME=/home/ubuntu/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
export STORM_HOME=/home/ubuntu/apache-storm
export PATH=$PATH:$STORM_HOME/bin
```

### Verify if the PATH is correctly setup

```
ubuntu@nimbus:~$ echo $STORM_HOME
/home/ubuntu/apache-storm

ubuntu@nimbus:~$ ls -l $STORM_HOME/bin
total 72
-rwxr-xr-x 1 ubuntu ubuntu  4214 Jul 27  2017 flight.bash
-rwxr-xr-x 1 ubuntu ubuntu  2256 Jul 27  2017 storm
-rwxr-xr-x 1 ubuntu ubuntu  9192 Jul 27  2017 storm.cmd
-rwxr-xr-x 1 ubuntu ubuntu  4417 Jul 27  2017 storm-config.cmd
-rwxr-xr-x 1 ubuntu ubuntu  1712 Jul 27  2017 storm-kafka-monitor
-rwxr-xr-x 1 ubuntu ubuntu 34823 Jul 27  2017 storm.py
```


### Storm configurations
> NOTE: By default everything is commented in storm configuration file

`vim apache-storm/conf/storm.yaml`

Add/edit the below parameters:

```
storm.zookeeper.servers:
     - "localhost"

storm.zookeeper.port: 2181
nimbus.host: localhost
```

> Results:
```
ubuntu@nimbus:~/apache-storm$ grep -v "^#" conf/storm.yaml

storm.zookeeper.servers:
     - "localhost"

storm.zookeeper.port: 2181
nimbus.host: localhost
```

### Add the `JAVA_HOME` in `storm-env.sh` file
`vim conf/storm-env.sh`

> Results:
```
ubuntu@nimbus:~/apache-storm$ grep JAVA_HOME conf/storm-env.sh
#export JAVA_HOME=/path/to/jdk/home
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```


### Before start Storm service please make sure Zookeeper is running
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
19195 QuorumPeerMain
19420 Jps
```

### Start Nimbus
`storm nimbus &`

> Results:
```
ubuntu@nimbus:~/apache-storm$ storm nimbus &
[1] 19932
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
20058 Jps
19195 QuorumPeerMain
19932 nimbus
```

### Start Supervisor
`storm supervisor &`

> Results:
```
ubuntu@nimbus:~/apache-storm$ storm supervisor &
[2] 20071
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
20071 Supervisor
19195 QuorumPeerMain
19932 nimbus
20173 Jps
```

### Start UI
`storm ui &`

> Results:
```
ubuntu@nimbus:~/apache-storm$ storm ui &
[3] 20185
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
ubuntu@nimbus:~/apache-storm$ jps
20278 Jps
20071 Supervisor
20185 core
19195 QuorumPeerMain
19932 nimbus
```


### Verify the Nimbus UI
Open the Nimbus UI in browser at port 8080: 
[Nimbus UI](http://192.168.1.11:8080)

Next: [Storm Multinode](2-storm-multinode.md)
