# Cấu hình HA cho MariaDB

## Gồm 3 server:
- **server-1**: 192.168.47.121
- **server-2**: 192.168.47.122
- **server-3**: 192.168.47.123

---

## 1. Cài đặt các công cụ cần thiết (trên cả 3 server)
```bash
apt install -y net-tools telnet traceroute
apt install -y mariadb-server mariadb-client
```

---

## 2. Cấu hình Galera Cluster
Mở tệp cấu hình `/etc/mysql/conf.d/galera.cnf`.

### 2.1 Server-1
```ini
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Cluster Configuration
wsrep_cluster_name="mariadb-cluster-devopseduvn"
wsrep_cluster_address="gcomm://192.168.47.121,192.168.47.122,192.168.47.123"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Node Configuration
wsrep_node_address="192.168.47.121"
wsrep_node_name="server-1"
```

### 2.2 Server-2
```ini
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Cluster Configuration
wsrep_cluster_name="mariadb-cluster-devopseduvn"
wsrep_cluster_address="gcomm://192.168.47.121,192.168.47.122,192.168.47.123"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Node Configuration
wsrep_node_address="192.168.47.122"
wsrep_node_name="server-2"
```

### 2.3 Server-3
```ini
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Cluster Configuration
wsrep_cluster_name="mariadb-cluster-devopseduvn"
wsrep_cluster_address="gcomm://192.168.47.121,192.168.47.122,192.168.47.123"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Node Configuration
wsrep_node_address="192.168.47.123"
wsrep_node_name="server-3"
```

---

## 3. Cấu hình MariaDB Galera Cluster

### Tắt MariaDB Server
```bash
systemctl stop mariadb
```

### Khởi tạo cụm MariaDB Galera Cluster trên server-1
```bash
galera_new_cluster
```

### Kiểm tra cụm MariaDB Galera Cluster (số lượng nút = 1)
```bash
mysql -u root -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
```

### Trên server-2 và server-3
Khởi động lại MariaDB:
```bash
systemctl restart mariadb
```

### Kiểm tra cụm (số lượng nút = 3)
```bash
mysql -u root -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
```

---

## 4. Kiểm tra tính đồng bộ của cụm MariaDB

### Thực hiện trên server-1
```sql
CREATE DATABASE devopseduvndb;
USE devopseduvndb;
CREATE TABLE series (Name VARCHAR(50) PRIMARY KEY, Lever VARCHAR(25), Release_date DATE);
INSERT INTO series (Name, Lever, Release_date) VALUES
('DevOps for Freshers', 'Basic/Freshers', '2024-01-15'),
('Xây dựng quy trình pipeline DevSecOps', 'Thực tế chuyên sâu', '2024-06-16'),
('HA tools', 'Advanced/Thực tế', '2024-08-18');
```

### Kiểm tra trên server-2 và server-3
```sql
USE devopseduvndb;
SHOW DATABASES;
SHOW TABLES;
SELECT * FROM series;
```

---

## 5. Cài đặt MaxScale trên cả 3 server

### Tải và cài đặt MaxScale
```bash
wget https://dlm.mariadb.com/2344079/MaxScale/6.4.1/packages/ubuntu/jammy/x86_64/maxscale-6.4.1-1.ubuntu.jammy.x86_64.deb
dpkg -i maxscale-6.4.1-1.ubuntu.jammy.x86_64.deb
```

### Sao lưu và cấu hình MaxScale
```bash
cp /etc/maxscale.cnf /etc/maxscale.cnf.org
vim /etc/maxscale.cnf
```

#### Nội dung cấu hình MaxScale
```ini
# MaxScale documentation:
# https://mariadb.com/kb/en/mariadb-maxscale-6/

[maxscale]
threads=auto

# Server definitions
[server1]
type=server
address=192.168.47.121
port=3306
protocol=MariaDBBackend

[server2]
type=server
address=192.168.47.122
port=3306
protocol=MariaDBBackend

[server3]
type=server
address=192.168.47.123
port=3306
protocol=MariaDBBackend

# Monitor for the servers
[MariaDB-Monitor]
type=monitor
module=mariadbmon
servers=server1,server2,server3
user=maxscale 
password=devopseduvn
monitor_interval=2000

# Service definitions
[Read-Only-Service]
type=service
router=readconnroute
servers=server1
# Tạo user trong database
user=maxscale 
# Pass của user
password=devopseduvn
router_options=slave

[Read-Write-Service]
type=service
router=readwritesplit
servers=server1,server2,server3
user=maxscale
password=devopseduvn

# Listener definitions
[Read-Only-Listener]
type=listener
service=Read-Only-Service
protocol=MariaDBClient
port=4008

[Read-Write-Listener]
type=listener
service=Read-Write-Service
protocol=MariaDBClient
port=4006
```

### Cấu hình cho phép truy cập từ mọi IP
```bash
vim /etc/mysql/mariadb.conf.d/50-server.cnf
```
Chỉnh sửa dòng:
```ini
bind-address = 0.0.0.0
```

### Tạo user và password trên server-1
```sql
CREATE USER 'maxscale'@'%' IDENTIFIED BY 'devopseduvn';
GRANT REPLICATION CLIENT ON *.* TO 'maxscale'@'%';
GRANT REPLICATION SLAVE ADMIN ON *.* TO 'maxscale'@'%';
GRANT REPLICA MONITOR ON *.* TO 'maxscale'@'%';
FLUSH PRIVILEGES;
```

### Kiểm tra trạng thái MaxScale
```bash
maxctrl list servers
```

---

## 6. Tài liệu tham khảo
- [How to Set Up MariaDB Galera Clusters on Ubuntu](https://www.linode.com/docs/guides/how-to-set-up-mariadb-galera-clusters-on-ubuntu-2204/)
- [Setting Up MariaDB Galera and MaxScale](https://thushan93515.medium.com/setting-up-mariadb-galera-and-maxscale-d42ff8307a66)
