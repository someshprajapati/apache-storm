# Multi Node Storm Installation and Configurations


# Storm setup on Storm2(supervisor-1) and Storm3(supervisor-2) servers
Follow the same steps on Storm2(supervisor-1) and Storm3(supervisor-2) servers which we followed on Storm1(nimbus) server previously till Step[`Verify if the PATH is correctly setup`]: [storm-standalone](1-storm-standalone.md)


### Storm configurations
> NOTE: By default everything is commented in storm configuration file

`vim apache-storm/conf/storm.yaml`

Add/edit the below parameters:

```
storm.zookeeper.servers:
     - "nimbus"
     - "supervisor-1"
     - "supervisor-2"

storm.zookeeper.port: 2181
nimbus.host: localhost
```

> Results:
```
ubuntu@nimbus:~/apache-storm$ grep -v "^#" conf/storm.yaml

storm.zookeeper.servers:
     - "nimbus"
     - "supervisor-1"
     - "supervisor-2"

storm.zookeeper.port: 2181
nimbus.host: nimbus


ubuntu@supervisor-1:~/apache-storm$ grep -v "^#" conf/storm.yaml

storm.zookeeper.servers:
     - "nimbus"
     - "supervisor-1"
     - "supervisor-2"

storm.zookeeper.port: 2181
nimbus.host: supervisor-1


ubuntu@supervisor-2:~/apache-storm$ grep -v "^#" conf/storm.yaml

storm.zookeeper.servers:
     - "nimbus"
     - "supervisor-1"
     - "supervisor-2"

storm.zookeeper.port: 2181
nimbus.host: supervisor-2
```

### Add the `JAVA_HOME` in `storm-env.sh` file
`vim conf/storm-env.sh`

> Results:
```
ubuntu@nimbus:~/apache-storm$ grep JAVA_HOME conf/storm-env.sh
#export JAVA_HOME=/path/to/jdk/home
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

ubuntu@supervisor-1:~/apache-storm$ grep JAVA_HOME conf/storm-env.sh
#export JAVA_HOME=/path/to/jdk/home
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

ubuntu@supervisor-2:~/apache-storm$ grep JAVA_HOME conf/storm-env.sh
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

ubuntu@supervisor-1:~/apache-storm$ jps
19637 QuorumPeerMain
19766 Jps

ubuntu@supervisor-2:~/apache-storm$ jps
19666 Jps
19547 QuorumPeerMain
```

### Start Nimbus on Storm1(Nimbus Server)
`storm nimbus &`

> Results:
```
ubuntu@nimbus:~/apache-storm$ storm nimbus &
[1] 20830
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
20944 Jps
20830 nimbus
20479 QuorumPeerMain
```

### Start Supervisor on Storm2 and Storm3(Supervisor Server)
`storm supervisor &`

> Results:
```
ubuntu@supervisor-1:~/apache-storm$ storm supervisor &
[1] 19778

ubuntu@supervisor-2:~/apache-storm$ storm supervisor &
[1] 19678
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@supervisor-1:~/apache-storm$ jps
19778 Supervisor
19637 QuorumPeerMain
19883 Jps


ubuntu@supervisor-2:~/apache-storm$ jps
19783 Jps
19547 QuorumPeerMain
19678 Supervisor
```

### Start UI on Storm1(Nimbus Server)
`storm ui &`

> Results:
```
ubuntu@nimbus:~/apache-storm$ storm ui &
[2] 20959
```

> NOTE: wait for few minute and check the status
`jps`

> Results:
```
ubuntu@nimbus:~/apache-storm$ jps
21052 Jps
20830 nimbus
20479 QuorumPeerMain
20959 core
```


### Verify the Nimbus UI
Open the Nimbus UI in browser at port 8080: 
[Nimbus UI](http://192.168.1.11:8080)
