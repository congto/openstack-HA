

## C. Xử lý lỗi và các lệnh kiểm tra
### C.1. Xử lý tình huống các node galera không start được
- Log sau khi các 3 node bị reboot đồng thời: http://paste.openstack.org/raw/609728/
- Hoặc kiểm tra trạng thái của mariadb sẽ có kết quả: http://paste.openstack.org/raw/609729/
- Xử lý lỗi khi reboot đồng thời cụm database - galera (reboot mà ko chờ các node up).
  - Xem nội dung file `/var/lib/mysql/grastate.dat` trên tất cả các máy trong cluster, máy nào có dòng `safe_to_bootstrap:` với giá trị lớn hơn (thường là `1`) thì thực hiện lệnh
  
    ```sh
    galera_new_cluster
    ```
  - Sau khi xử lý xong sẽ có log ở `/var/log/messages` như sau: http://paste.openstack.org/raw/609730/
- Tiếp tục thực hiện restart mariadb trên các node còn lại để join cluster ` systemctl restart mariadb`
- Sau khi join lại xong, kiểm tra bằng lệnh sau để xem số node trong cluter đã ok hay chưa ()
  ```sh
  mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
  ```
  - Kết quả lệnh trên như sau:
    ```sh
    [root@db3 ~]# mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
    Enter password:
    +--------------------+-------+
    | Variable_name      | Value |
    +--------------------+-------+
    | wsrep_cluster_size | 3     |
    +--------------------+-------+
    [root@db3 ~]#
    ```

### Khác
``
`
if [ "$1" == "controller" ]; then
      echo "$HOST_CTL" > $path_hostname
      hostname -F $path_hostname
  
  elif [ "$1" == "compute1" ]; then
      echo "$HOST_COM1" > $path_hostname
      hostname -F $path_hostname

  elif [ "$1" == "compute2" ]; then
      echo "$HOST_COM2" > $path_hostname
      hostname -F $path_hostname
  else
      echocolor "Cau hinh hostname that bai"
      echocolor "Cau hinh hostname that bai"
      exit 1
fi

echo "NAME=ens160" >> /etc/sysconfig/network-scripts/ifcfg-ens160
nmcli con reload
nmcli con show
curl -O https://raw.githubusercontent.com/congto/openstack-HA/master/scripts/rabbitmq-bonding.sh


sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

curl -O https://raw.githubusercontent.com/congto/openstack-HA/master/scripts/rabbitmq1-ipadd.sh && bash rabbitmq1-ipadd.sh

- Đổi tên connection 
nmcli c modify "ens1" connection.id ens160

160 -> 192 -> 224 -> 256 -> 161 -> 193 -> 225 -> 257 -> 163 -> 194 -> 226 -> 258

ens160 -> ens192
ens224 -> ens256
ens161 -> ens193 
ens225 -> ens257
ens163 -> ens194
ens226 -> ens258

### 
Chay script bonding tren cac may LB

bash lb-bonding.sh lb1 10.10.20.31 10.10.10.31 192.168.20.31 192.168.40.31
bash lb-bonding.sh lb2 10.10.20.32 10.10.10.32 192.168.20.32 192.168.40.32


### Ghi chep ve add resource cho pacemaker
echo IP_VIP_API=10.10.20.30 >> lb-config.cfg 
echo IP_VIP_DB=10.10.10.30 >> lb-config.cfg 
echo IP_VIP_ADMIN=192.168.20.30 >> lb-config.cfg 

pcs resource create Virtual_IP_API ocf:heartbeat:IPaddr2 ip=10.10.20.30 cidr_netmask=32 op monitor interval=30s
pcs resource create Virtual_IP_MNGT ocf:heartbeat:IPaddr2 ip=192.168.20.30 cidr_netmask=32 op monitor interval=30s
pcs resource create Virtual_IP_DB ocf:heartbeat:IPaddr2 ip=10.10.10.30 cidr_netmask=32 op monitor interval=30s
pcs resource create Web_Cluster ocf:heartbeat:nginx configfile=/etc/nginx/nginx.conf status10url op monitor interval=5s

pcs constraint colocation add Web_Cluster with Virtual_IP_API INFINITY        
pcs constraint colocation add Web_Cluster with Virtual_IP_DB INFINITY     
pcs constraint colocation add Web_Cluster with Virtual_IP_MNGT INFINITY     

pcs constraint order Virtual_IP_API then Web_Cluster
pcs constraint order Virtual_IP_API then Web_Cluster
pcs constraint order Virtual_IP_MNGT then Web_Cluster   


# pcs resource clone Web_Cluster globally-unique=true clone-max=2 clone-node-max=2
pcs constraint colocation add Web_Cluster with Virtual_IP_API INFINITY        
pcs constraint order Virtual_IP_API then Web_Cluster
pcs constraint order Virtual_IP_DB then Web_Cluster



#### Node keystone

 mysql+pymysql://keystone:KEYSTONE_DBPASS@controller/keystone

PASS_DATABASE_KEYSTONE='Ec0net@!20171
mysql+pymysql://keystone:Ec0net@!20171@10.10.10.30/keystone


crudini --set /etc/keystone/keystone.conf database connection mysql+pymysql://keystone:'Ec0net@!2017'@10.10.10.30/keystone
crudini --set /etc/keystone/keystone.conf database connection mysql+pymysql://keystone:'Ec0net@!2017'@10.10.10.53/keystone

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Ec0net@!2017' WITH GRANT OPTION ;FLUSH PRIVILEGES;

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '$PASS_DATABASE_ROOT' ;FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY '$PASS_DATABASE_ROOT';FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.20.61' IDENTIFIED BY '$PASS_DATABASE_ROOT' WITH GRANT OPTION ;FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.20.62' IDENTIFIED BY '$PASS_DATABASE_ROOT' WITH GRANT OPTION ;FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.20.63' IDENTIFIED BY '$PASS_DATABASE_ROOT' WITH GRANT OPTION ;FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'127.0.0.1' IDENTIFIED BY '$PASS_DATABASE_ROOT';FLUSH PRIVILEGES;


GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'localhost' IDENTIFIED BY 'Welcome123' WITH GRANT OPTION; FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' IDENTIFIED BY 'Welcome123' WITH GRANT OPTION; FLUSH PRIVILEGES;


GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' IDENTIFIED BY 'Ec0net@!2017' WITH GRANT OPTION; FLUSH PRIVILEGES;
FLUSH PRIVILEGES;"


PASS_DATABASE_ROOT='Ec0net@!2017'
PASS_DATABASE_KEYSTONE=PASS_DATABASE_ROOT
DB1_IP_NIC2=192.168.20.51
mysql -uroot -p$PASS_DATABASE_ROOT -h $DB1_IP_NIC2 -e "CREATE DATABASE keystone;

CREATE DATABASE keystone;
GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'localhost' IDENTIFIED BY 'Ec0net#!2017';
GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' IDENTIFIED BY 'Ec0net#!2017';
FLUSH PRIVILEGES;

"

PASS_DATABASE_KEYSTONE='Ec0net@!2017'


slmgr /skms active.orientsoftware.asia
slmgr /ato

http://prntscr.com/f93zk6

connection = mysql+pymysql://keystone:Ec0net@!2017@10.10.10.30/keystone

su -s /bin/sh -c "keystone-manage db_sync" keystone

keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
keystone-manage credential_setup --keystone-user keystone --keystone-group keystone


keystone-manage bootstrap --bootstrap-password Welcome123 \
  --bootstrap-admin-url http://10.10.20.61:35357/v3/ \
  --bootstrap-internal-url http://10.10.20.61:35357/v3/ \
  --bootstrap-public-url http://10.10.20.61:5000/v3/ \
  --bootstrap-region-id RegionOne



keystone-manage bootstrap --bootstrap-password Welcome123 \
  --bootstrap-admin-url http://10.10.20.6:35357/v3/ \
  --bootstrap-internal-url http://10.10.20.30:35357/v3/ \
  --bootstrap-public-url http://10.10.20.30:5000/v3/ \
  --bootstrap-region-id RegionOne
  
  
export OS_USERNAME=admin
export OS_PASSWORD=Ec0net#!2017
export OS_PROJECT_NAME=admin
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_AUTH_URL=http://10.10.20.30:35357/v3
export OS_IDENTITY_API_VERSION=3

openstack project create --domain default --description "Service Project" service


systemctl enable httpd.service
systemctl start httpd.service
systemctl restart httpd.service





