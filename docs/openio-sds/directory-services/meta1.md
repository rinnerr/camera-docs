•	Thư mục Meta1 lưu địa chỉ Meta2 cho mỗi container (bucket). Thư mục Meta1 có thể quản lý hàng triệu container.  
•   Phải được triển khai trên 3 servers khác nhau  
•	Nên triển khai trên high performance storage như SSD hay NVMe

Meta1-service:

    # OpenIO managed
    [Unit]
    Description=OPENIO-meta1-0
    After=network-online.target
    Wants=network-online.target

    [Service]
    User=openio
    Group=openio
    ExecStart=/usr/bin/oio-meta1-server -p /run/oio/sds/OPENIO-meta1-0.pid -s OIO,OPENIO,meta1,0 -O Endpoint=192.168.2.35:6110  -O Config=/etc/oio/sds/OPENIO/meta1-0/meta1-0.conf OPENIO /var/lib/oio/sds/OPENIO/meta1-0
    Restart=always

    [Install]
    WantedBy=multi-user.target
