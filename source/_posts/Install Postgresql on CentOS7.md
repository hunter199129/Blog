---
title: 'Install Postgresql on CentOS7'
date: 2018-08-15 11:13:00
tags: [Postgres, CentOS, Database]
categories:Devops
---

這次在Linux裝**postgres**的時候踩了一些小坑

所以就想說把這些資訊整理整理，以便之後查看

<!--More-->

## Download Rpm Package and Install

Refer to [postgres official website](https://www.postgresql.org/download/linux/redhat/).
[Here](https://yum.postgresql.org/rpmchart.php) can download postgres package

I use Packages Groups: *PostgreSQL Database Server 10 PGDG*

+ postgresql10 - *PostgreSQL client programs and libraries*
+ postgresql10-contrib - *Contributed source and binaries distributed with PostgreSQL*
+ postgresql10-libs - *The shared libraries required for any PostgreSQL clients*
+ postgresql10-server - *The programs needed to create and run a PostgreSQL server*

and then yum install all the things

## Setup Postgres Service

    postgresql-setup initdb
    systemctl enable postgresql.service
    systemctl start postgresql.service

Now postgres server should be on and is accessible at local side (we'll configure the server to let remote side accessible).

## Login to Postgres and Set postgres Password

Postgres localhost login is default using '**peer**' method, so it doesn't required password to login

    sudo -u postgres psql

It should be in psql client now, changing password command using

    postgres=# ALTER USER postgres PASSWORD 'myPassword';
    ALTER ROLE

after change the password using

    \q

to quit the psql client

## About Default Login approach: peer and ident 

> Firstly, it is important to understand that for most Unix distributions, the default Postgres user neither requires nor uses a password for authentication. Instead, depending how Postgres was originally installed and what version you are using, the default authentication method will either be ident or peer.  
> **ident** authentication uses the operating system’s identification server running at TCP port 113 to verify the user’s credentials.  
> **peer** authentication on the other hand, is used for local connections and verifies that the logged in username of the operating system matches the username for the Postgres database.

ref: https://chartio.com/resources/tutorials/how-to-set-the-default-user-password-in-postgresql/

## Access Postgres by Pgadmin from remote

### Setting pg_hba.conf

**pg_hba.conf** is under `/var/lib/pgsql/<version_of_postgres>/data/` in CentOS by default

we need to change the **ipv4 local connections** item:

    host    all             all             0.0.0.0/0               md5

ip `0.0.0.0/0` will let any ip address can access the server  
using **md5** to access postgres by login using password, check here [19.3. Authentication Methods](https://www.postgresql.org/docs/9.1/static/auth-methods.html)

### Setting postgresql.conf

Open listen_addresses and port

    listen_addresses = '*' 
    port = 5432

after this your need to **reboot** the server, and using

    netstat -nplt | grep 5432

or 

    netstat -a | grep PGSQL

to check if the port is listening by server

### Setting firewall

Use

    sudo firewall-cmd --zone=public --permanent --add-port=5432/tcp

to open tcp 5432 port permanently, to check the setting:

    sudo firewall-cmd --zone=public --list-all

---

now you should be able to access postgres by using pgadmin and not encounter
> could not connect to server: Connection refused (0x0000274C/10060)

and 

> could not connect to server: Connection refused (0x0000274D/10061)
