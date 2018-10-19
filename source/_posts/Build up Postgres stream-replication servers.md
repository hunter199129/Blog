---
title: 'Build up Postgres stream-replication servers.md'
date: 2018-10-19 15:37:00
tags: [Postgres, docker]
categories: Database
---

Postgres 的 Stream Replication 是內建的強大功能

不過設定的時候踩了一些坑，記錄一下步驟

不過 WAL 強大的功能還沒有用到，之後再把 WAL 的功能試一試

<!--More-->

要設定的主要有幾個檔案:

1. **postgresql.conf** : postgresql 主要的設定檔
2. **pg_hba.conf** : 要連進 postgresql 的方式
3. **recovery.conf** : 設定 slave node 最重要的file， master node 不需設定這個檔案
    
可以利用 sample file 更改設定， sample file 的位置通常在 `/usr/share/postgres/`

若是用 docker image 可以用以下指令把 config 拉到 host 中

    docker run -i --rm postgres cat /usr/share/postgresql/postgresql.conf.sample > my-postgres.conf

## Basic settings

### postgresql.conf

更改下列參數

#### For master node:

    # 讓任何 ip 都可以存取
    listen_addresses = '*'
    
    wal_level = replica

    # 開啟 archive mode 並設定 archive 位置
    archive_mode = on
    archive_command = 'cp -a "%p" /var/lib/postgresql/archivedir/"%f"'  

    # wal senders 的數量限制
    max_wal_senders = 5
    # wal segements 大小限制
    wal_keep_segments = 32

#### For slave node:

    listen_addresses = '*'

    wal_level = replica

    # 允許 recovery 時 query
    hot_standby = on
    max_standby_archive_delay = 30s
    max_standby_streaming_delay = 30s

> Note: 若是需要建構多台，可以作模組化，只要在最後加上檔案名稱即可

例如建一個 `slave.conf` 然後再在 `postgresql.conf` 最後加上

    include = 'slave.conf'

### pg_hba.conf

Add

    host    all             all             all                     md5
    host    replication     postgres        all                     md5

> Note: replication database 跟 all 是分開的，所以一定要有

### recovery.conf

只有 Slave node 需要

建新檔案或是去改 `recovery.conf.sample` 都可以，因為只有三行

    standby_mode = on
    # 連到 master node 的 connection string
    primary_conninfo = 'host=172.17.0.3 port=5432 user=postgres password=postgres' 
    # 這邊建好 master node 時需要給 replication slot 對應的東西就是這個 slot name
    primary_slot_name = 'pg_slave_1'

## Build master node

用以上的設定建好 master node 重開之後，建立 replication slot

用 psql 下 sql command

    $ sudo -u postgres psql
    ## 對應到上面 slave configuration 的 slot name
    postgres=# SELECT * FROM pg_create_physical_replication_slot('pg_slave_1');

## Build slave node

Conf 檔設定完之後先不要開啟 server ，要先用`pg_basebackup` 去 sync master 的資料，否則會出現資料狀態不一致的 Error

    ## 記得先 check directory 的位置是否正確
    pg_basebackup -h 172.17.0.3 -p 5432 -D /var/lib/postgresql/data -P -U postgres -X stream
    chown -R postgres:postgres /var/lib/postgresql/data
    chmod -R 0700 /var/lib/postgresql/data

開啟 server 後就可以 check master and slave 是否同步了

## checking commands

- On master:

  select pg_current_wal_lsn();
  
- On slave:

  select pg_last_wal_replay_lsn();

## using docker

### primary:

    docker run -d --name postgres-5432 -p 5432:5432 -e POSTGRES_PASSWORD=postgres -v "$PWD/postgres-5432.conf":/etc/postgresql/postgresql.conf postgres -c 'config_file=/etc/postgresql/postgresql.conf'

    echo "host    replication     postgres        all                     md5" >> /var/lib/postgresql/data/pg_hba.conf

    pg_ctl restart

    SELECT * FROM pg_create_physical_replication_slot('pg_slave_1');

### slave:

    docker run -d --name postgres-5433 -p 5433:5433 -e POSTGRES_PASSWORD=postgres \
    -v "$PWD/postgres-5433.conf":/etc/postgresql/postgresql.conf \
    -v "$PWD/recovery.conf":/etc/postgresql/recovery.conf \
    postgres -c 'config_file=/etc/postgresql/postgresql.conf'

    pg_basebackup -h 172.17.0.3 -p 5432 -D /var/lib/postgresql/main -P -U postgres -X stream

    chown -R postgres:postgres /var/lib/postgresql/main
    chmod -R 0700 /var/lib/postgresql/main

    cp /etc/postgresql/recovery.conf /var/lib/postgresql/main/recovery.conf
    cp -rf /var/lib/postgresql/main/* /var/lib/postgresql/data

