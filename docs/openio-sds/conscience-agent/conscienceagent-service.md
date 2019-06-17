# OpenIO managed  
[Unit]  
Description=OPENIO-conscienceagent-0  
After=network-online.target  
Wants=network-online.target  

[Service]  
User=openio  
Group=openio  
ExecStart=/usr/bin/oio-conscience-agent /etc/oio/sds/OPENIO/conscienceagent-0/conscienceagent-0.yml  
Restart=always  

[Install]  
WantedBy=multi-user.target  

