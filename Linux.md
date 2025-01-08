# 修改root密码

```
sudo passwd root
su root
```

# 修改SSH登录

```
vim /etc/ssh/sshd_config
:set nu
? root
33 G
PermitRootLogin

systemctl restart ssh
```

# 设置静态IP

```
ip link show

cd /etc/netplan
ls -l

vim 50-cloud-init.yaml
```

```yaml
network:
    renderer: networkd
    ethernets:
        ens33:
            dhcp4: false
            dhcp6: false
            addresses: [192.168.10.10/24]
            routes:
              - to: default
                via: 192.168.10.1
            nameservers:
                addresses: [192.168.10.1]
                search: []
    version: 2
```

```
netplan apply

ip addr show
ip route show
ping www.baidu.com
```

# Mysql镜像

```
docker container run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql

docker container run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

docker exec -it mysql /bin/bash

use mysql;
select host,user from user;
```

