#oioproxy service    
    [Unit]  
    Description=OPENIO-oioproxy-0 daemon from OpenIO  
    After=network-online.target
    Wants=network-online.target  
  
    [Service]  
    User=openio  
    Group=openio  
    ExecStart=/usr/bin/oio-proxy -p /run/oio/sds/OPENIO-oioproxy-0.pid -s OIO,OPENIO,oioproxy,0 -O Config=/etc/oio/sds/OPENIO/oioproxy-0/oioproxy-0.conf 192.168.2.35:6006 OPENIO  
    Restart=always  
  
    [Install]  
    WantedBy=multi-user.target  
