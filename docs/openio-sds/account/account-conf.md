  Nằm trong /etc/oio/sds/account:   
    # OpenIO managed
    [account-server]
    bind_addr = 192.168.2.35
    bind_port = 6009
    #sentinel_hosts = 192.168.2.37:6012,192.168.2.36:6012,192.168.2.35:6012
    #sentinel_master_name = OPENIO-master-1
    log_level = INFO
    log_facility = LOG_LOCAL0
    log_address = /dev/log
    syslog_prefix = OIO,OPENIO,account,0
    autocreate = true
