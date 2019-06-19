•	Một tập hợp các network services kết hợp làm việc cùng nhau để chạy các giải pháp OpenIO.

    # OpenIO managed
    [OPENIO]
    # endpoints
    conscience=192.168.2.37:6000,192.168.2.36:6000,192.168.2.35:6000
    zookeeper=192.168.2.37:6005,192.168.2.36:6005,192.168.2.35:6005
    proxy=192.168.2.35:6006
    event-agent=beanstalk://192.168.2.35:6014
    ecd=192.168.2.35:6017

    udp_allowed=yes

    meta1_digits=2
    ns.meta1_digits=2
    ns.storage_policy=SINGLE
    ns.chunk_size=104857600
    ns.service_update_policy=meta2=KEEP|3|1|;rdir=KEEP|1|1|;

•	**proxy:** Thông báo cho client SDK trỏ về oio-proxy nào, là endpoint chính cho namespace.  
•	**conscience:** cho oio-proxy biết có những conscience nào đang được dùng.  
•	**zookeeper:** các zookeeper trong cluster, áp dụng cho các meta0, meta1, meta2 và sqlx services.  
•	**ecd:** cho client SDK service ecd đang chạy với ip và port nào.  
•	**event-agent:** event-agent connection dùng để tạo ra notifications. Bao gồm protocol và endpoint của notification backend (default beanstalk://)  
•	**ns.storage_policy:** phía storage có bao nhiêu replicate (SINGLE=1, TWOCOPIES=2, THREECOPIES=3)  
