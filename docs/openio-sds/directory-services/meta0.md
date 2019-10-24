•	Thư mục Meta0 lưu địa chỉ Meta1 cho mỗi container (bucket). Meta0 chỉ xử lý tối đa 65,536 container. Và chỉ có một Meta0 trên mỗi namespace.  
•   Phải được triển khai trên 3 servers khác nhau  
•	Nên triển khai trên high performance storage như SSD hay NVMe

Tạo Meta0-service trong /etc/systemd/system: 

    # OpenIO managed  
    [Unit]  
    Description=OPENIO-meta0-0  
    After=network-online.target  
    Wants=network-online.target  
    
    [Service]  
    User=openio  
    Group=openio  
    ExecStart=/usr/bin/oio-meta0-server -p /run/oio/sds/OPENIO-meta0-0.pid -s OIO,OPENIO,meta0,0 -O Endpoint=192.168.2.35:6001  -O             Config=/etc/oio/sds/OPENIO/meta0-0/meta0-0.conf OPENIO /var/lib/oio/sds/OPENIO/meta0-0  
    Restart=always  

    [Install]  
    WantedBy=multi-user.target  
