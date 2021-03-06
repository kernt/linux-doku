---
title: mariadb-maxscale
description: 
published: true
date: 2021-06-09T15:37:46.420Z
tags: 
editor: markdown
dateCreated: 2021-06-09T15:37:41.505Z
---

# maxscale Einrichtung

_/etc/maxscale.cnf_ Konfiguration einrichten

```sh
# Globals
[maxscale]
threads=1

# Servers
[server1]
type=server
address=192.168.0.181
port=3306
protocol=MySQLBackend

[server2]
type=server
address=192.168.0.182
port=3306
protocol=MySQLBackend

[server3]
type=server
address=192.168.0.183
port=3306
protocol=MySQLBackend

# Monitoring for the servers
[Galera Monitor]
type=monitor
module=galeramon
servers=server1,server2,server3
user=myuser
passwd=mypwd
monitor_interval=1000

# Galera router service
[Galera Service]
type=service
router=readwritesplit
servers=server1,server2,server3
user=myuser
passwd=mypwd

# MaxAdmin Service
[MaxAdmin Service]
type=service
router=cli

# Galera cluster listener
[Galera Listener]
type=listener
service=Galera Service
protocol=MySQLClient
port=3306

# MaxAdmin listener
[MaxAdmin Listener]
type=listener
service=MaxAdmin Service
protocol=maxscaled
socket=default
```

**Quellen:**

* [getting-started-with-mariadb-galera-and-mariadb-maxscale-on-centos](https://mariadb.com/resources/blog/getting-started-with-mariadb-galera-and-mariadb-maxscale-on-centos/)