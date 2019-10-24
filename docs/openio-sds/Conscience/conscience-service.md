    # OpenIO managed  
    [Unit]  
    Description=OPENIO-conscience-0 daemon from OpenIO  
    After=network-online.target  
    Wants=network-online.target  

    [Service]
    User=openio  
    Group=openio  
    ExecStart=/usr/bin/oio-daemon -s OIO,OPENIO,conscience,0 /etc/oio/sds/OPENIO/conscience-0/conscience-0.conf  
    Restart=always  

    [Install]  
    WantedBy=multi-user.target  

