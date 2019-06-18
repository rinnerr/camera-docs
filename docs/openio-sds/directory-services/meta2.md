•	Thư mục Meta2 lưu trữ danh sách nội dung cho từng container  và địa chỉ chunk cho từng phần nội dung.  
•   Phải được triển khai trên 3 servers khác nhau  
•	Nên triển khai trên high performance storage như SSD hay NVMe  

Meta2-service:  

    # OpenIO managed
    [Unit]
    Description=OPENIO-meta2-0
    After=network-online.target
    Wants=network-online.target

    [Service]
    User=openio
    Group=openio
    ExecStart=/usr/bin/oio-meta2-server -p /run/oio/sds/OPENIO-meta2-0.pid -s OIO,OPENIO,meta2,0 -O Endpoint=192.168.2.35:6120  -O Config=/etc/oio/sds/OPENIO/meta2-0/meta2-0.conf OPENIO /var/lib/oio/sds/OPENIO/meta2-0
    Restart=always

    [Install]
    WantedBy=multi-user.target
