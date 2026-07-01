# Disaster-recovery_put


# Задание 1

<img width="1527" height="733" alt="image" src="https://github.com/user-attachments/assets/c5a23fc4-7f2f-4a87-bdfb-7f70601914d9" />

Файл из циско прикреплен в репозитории
# Задание 2

Мастер:

<img width="672" height="240" alt="image" src="https://github.com/user-attachments/assets/7a79fd71-2a70-474c-aee5-98ff58623aa1" />

# Баш скрипт:
```
#!/bin/bash
if systemctl is-active --quiet nginx && [ -f /var/www/html/index.html ]; then
    exit 0
else
    exit 1
fi
```

Конфигурационный файл:

```
vrrp_script check_nginx {
    script "/usr/local/bin/check_nginx.sh"
    interval 3
    weight -20
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1234
    }
    virtual_ipaddress {
        192.168.1.100
    }
    track_script {
        check_nginx
    }
}
```
# БЭКАП

Конфигурационный файл:

```
vrrp_script check_nginx {
    script "/usr/local/bin/check_nginx.sh"
    interval 3
    weight -20
}

vrrp_instance VI_1 {
    state BACKUP
    interface enp0s3
    virtual_router_id 51
    priority 90
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1234
    }
    virtual_ipaddress {
        192.168.1.100
    }
    track_script {
        check_nginx
    }
}
```
