Tạo file account service trong /etc/systemd/system: 

    # OpenIO managed
    [Unit]
    Description=OPENIO-account-0
    After=network-online.target
    Wants=network-online.target

    [Service]
    User=openio
    Group=openio
    ExecStart=/usr/bin/oio-account-server /etc/oio/sds/OPENIO/account-0/account-0.conf
    Restart=always

    [Install]
    WantedBy=multi-user.target
