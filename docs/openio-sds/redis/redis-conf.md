• Đây là file cấu hình của Redis. Ở đây, "bind 192.168.2.35" chỉ ip public của server và "slaveof 192.168.2.37 6011" cho biết Redis trên server "192.168.2.35" đang nhận server "192.168.2.37" là master.  

    # OpenIO managed
    daemonize no
    pidfile "/run/redis/OPENIO/redis-0/redis-0.pid"
    port 6011
    tcp-backlog 511
    bind 192.168.2.35
    timeout 0
    tcp-keepalive 0
    loglevel notice
    logfile "/var/log/oio/sds/OPENIO/redis-0/redis-0.log"
    databases 16
    save 900 1
    save 300 10
    save 60 10000
    stop-writes-on-bgsave-error yes
    rdbcompression yes
    rdbchecksum yes
    dbfilename "dump.rdb"
    dir "/var/lib/oio/sds/OPENIO/redis-0"
    slave-serve-stale-data yes
    slave-read-only yes
    slave-priority 100
    repl-diskless-sync no
    repl-diskless-sync-delay 5
    repl-disable-tcp-nodelay no
    appendonly no
    appendfilename "appendonly.aof"
    appendfsync everysec
    no-appendfsync-on-rewrite no
    auto-aof-rewrite-percentage 100
    auto-aof-rewrite-min-size 64mb
    aof-load-truncated yes
    lua-time-limit 5000
    slowlog-log-slower-than 10000
    slowlog-max-len 128
    latency-monitor-threshold 0
    notify-keyspace-events ""
    hash-max-ziplist-entries 512
    hash-max-ziplist-value 64
    list-max-ziplist-entries 512
    list-max-ziplist-value 64
    set-max-intset-entries 512
    zset-max-ziplist-entries 128
    zset-max-ziplist-value 64
    hll-sparse-max-bytes 3000
    activerehashing yes
    client-output-buffer-limit normal 0 0 0
    client-output-buffer-limit slave 256mb 64mb 60
    client-output-buffer-limit pubsub 32mb 8mb 60
    hz 10
    aof-rewrite-incremental-fsync yes
    maxclients 4064
    slaveof 192.168.2.37 6011
