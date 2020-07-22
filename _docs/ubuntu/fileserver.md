---
title: "fileserver"
permalink: /docs/ubuntu/fileserver/
toc: true
---

# Фйаловый серевр с бакетом Object Storage

## Цель

Создать ВМ с монтированным бакетом Object Storage через fuse,
который в совою очередь можно присоеденять как NFS сетевой диск,
для выполнения backup.

## Ссылки

- Инстркция по созданию файлового сервера
  (ссылка)[https://cloud.yandex.ru/docs/solutions/archive/single-node-file-server]
- Инстркция по монтированию бакета Object Storage
  (ссылка)[https://cloud.yandex.ru/docs/storage/tools/s3fs]

## Установка mc и nano

```
sudo apt-get update
sudo apt-get install mc nano
```

## Создаем папку для монитрования
`sudo mkdir /mnt`
`sudo mkdir /mnt/obj`

## Установливаем Samba

```
sudo apt-get install nfs-kernel-server samba
```

### Конфигурируем NFS в файле
Запускаем
```
sudo nano /etc/exports
```
Дописываем разрешения
```
/mnt/obj <IP-Adres>(rw,no_subtree_check,fsid=100)
/mnt/obj 127.0.0.1(rw,no_subtree_check,fsid=100)
```
где <IP-Adres> - адреса с которых будем подключать диск

Например:
```
/mnt/obj 10.130.0.3(rw,no_subtree_check,fsid=100)
/mnt/obj 10.128.0.3(rw,no_subtree_check,fsid=100)
/mnt/obj 127.0.0.1(rw,no_subtree_check,fsid=100)
```

## Конфигурируем Samba

```
sudo nano /etc/samba/smb.conf
````

Файл должен принять следющий вид:
```
[global]
   workgroup = WORKGROUP
   server string = %h server (Samba)
   dns proxy = no
   log file = /var/log/samba/log.%m
   max log size = 1000
   syslog = 0
   panic action = /usr/share/samba/panic-action %d
   server role = standalone server
   passdb backend = tdbsam
   obey pam restrictions = yes
   unix password sync = yes
   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes
[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700
[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
[data]
   comment = /data
   path = /mnt/obj
   browseable = yes
   read only = no
   writable = yes
   guest ok = yes
   hosts allow = 10.130.0.3 10.128.0.3 127.0.0.1
   hosts deny = 0.0.0.0/0
```

### Раздача прав
Дадим полные права
```
sudo chmod -R 777 /mnt
```

### Перезапускаем службы
Перезапукс nfs-kernel
```
sudo service nfs-kernel-server restart
```
Перезапукс smbd
```
sudo service smbd restart
```

## Монтируем диск object storage

[manpages.ubuntu.com](http://manpages.ubuntu.com/manpages/focal/man1/s3fs.1.html)

### Установливаем s3fs
s3fs — программа для Linux позволяющя монтировать объекты Object Storage
```
sudo apt install s3fs
```

### Создаем сервисный аккаунт
https://console.cloud.yandex.ru/folders/b1gdf022fs6ma7rj92d8?section=service-accounts
с ролями:
- storage.viewer
- storage.uploader

### Сохраняем серетный ключь в файл
Выполняем скрипт
Вариант 1. `/root/.passwd-s3fsre`
```
echo <идентификатор ключа>:<секретный ключ> >  /root/.passwd-s3fs
chmod 600  /root/.passwd-s3fsre
```
Вариант 2. `/etc/passwd-s3fs`
```
echo <идентификатор ключа>:<секретный ключ> >  /etc/passwd-s3fs
chmod 600  /etc/passwd-s3fs
```

### Добавляем в автозагрузку
Добавляем запись в /etc/fstab
```
sudo echo "s3fs#arctl-backup-files /mnt/obj fuse _netdev,allow_other,use_path_request_style,url=http://storage.yandexcloud.net 0 0" | sudo tee -a /etc/fstab
```
Важно! В этом случае пароль должен храниться в папке `/root/.passwd-s3fs`

Вариант 2
```
sudo echo "arctl-backup-files /mnt/obj fuse.s3fs _netdev,allow_other,use_path_request_style,url=http://storage.yandexcloud.net 0 0" | sudo tee -a /etc/fstab
```
Важно! В этом случае пароль должен храниться в папке `/etc/passwd-s3fs`

Проверяем добавление
`sudo nano /etc/fstab`

Вариант 2. С размещение кэша на дркугом диске -o use_cache
```
sudo echo "s3fs#arctl-backup-files /mnt/obj fuse _netdev,allow_other,use_cache=/mnt/vdb,use_path_request_style,url=http://storage.yandexcloud.net 0 0" | sudo tee -a /etc/fstab
```

### Монтируем диск

```
sudo s3fs arctl-backup-files /mnt/obj -o passwd_file=~/.passwd-s3fs \
    -o url=http://storage.yandexcloud.net -o use_path_request_style
```

Вариант 2. С размещение кэша на дркугом диске -o use_cache
```
sudo s3fs arctl-backup-files /mnt/obj -o passwd_file=~/.passwd-s3fs \
    -o use_cache=/mnt/vdb -o url=http://storage.yandexcloud.net -o use_path_request_style
```

## Подключение к диску
windows
```
net use x: \\<публичный IP-адрес виртуальной машины>\data
```
