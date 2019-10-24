•	Service Zookeeper được dùng để lưu trữ trạng thái lựa chọn của các services Meta0, Meta1, Meta2.  
•	Yêu cấu cần RAM cao.  

File cấu hình của zookeeper: zoo.cfg  

    # OpenIO managed
    # The number of milliseconds of each tick
    tickTime=2000
    # The number of ticks that the initial
    # synchronization phase can take
    initLimit=10
    # The number of ticks that can pass between
    # sending a request and getting an acknowledgement
    syncLimit=5
    # the directory where the snapshot is stored.
    dataDir=/var/lib/oio/sds/OPENIO/zookeeper-0
    # the port at which the clients will connect
    clientPort=6005
    maxClientCnxns=200
    autopurge.snapRetainCount=3
    autopurge.purgeInterval=1

    server.1=192.168.2.37:2888:3888
    server.2=192.168.2.36:2888:3888
    server.3=192.168.2.35:2888:3888
    
Tạo zookeeper-service trong /etc/systemd/system:   

    # OpenIO managed  
    [Unit]  
    Description=OPENIO-zookeeper-0  
    After=network-online.target  
    Wants=network-online.target  

    [Service]  
    User=openio  
    Group=openio  
    ExecStart=/usr/bin/java -Xms919M  -Xmx919M  -XX:+UseParallelGC -XX:ParallelGCThreads=1 -Djute.maxbuffer=8388608 -Dzookeeper.log.dir=/var/log/oio/sds/OPENIO/zookeeper-0 -Dzookeeper.root.logger=INFO,ROLLINGFILE -cp /etc/oio/sds/OPENIO/zookeeper-0:/usr/share/zookeeper/* -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.local.only=false org.apache.zookeeper.server.quorum.QuorumPeerMain /etc/oio/sds/OPENIO/zookeeper-0/zoo.cfg  
    Restart=always  

    [Install]  
    WantedBy=multi-user.target  
