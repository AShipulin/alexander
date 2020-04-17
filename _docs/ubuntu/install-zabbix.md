# Установка (install) Zabbix

Устанавливаем Zabbix с использованием:
- nginx
- PostgreSQL

[Документация на .zabbix.co](https://www.zabbix.com/ru/download?zabbix=4.4&os_distribution=ubuntu&os_version=18.04_bionic&db=postgresql)

## Подготовка
```
sudo apt-get update    
sudo apt-get install mc
sudo apt-get install nano
sudo mc
sudo timedatectl set-timezone Asia/Yekaterinburg
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
sudo apt install traceroute
```

## Установите репозиторий Zabbix:
```
sudo wget https://repo.zabbix.com/zabbix/4.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.4-1+bionic_all.deb
sudo dpkg -i zabbix-release_4.4-1+bionic_all.deb
sudo apt update
```

### Установка apache2, php:
```
# sudo apt install nginx php-fpm
sudo apt install php
sudo apt -y install zabbix-nginx-conf
sudo apt -y install zbx_monitor
sudo apt install
```

## Установите Zabbix сервер, веб-интерфейс и агент:
```
sudo apt -y install zabbix-server-pgsql zabbix-frontend-php zabbix-nginx-conf php-pgsql zabbix-agent
```


## Создайте базу данных в PostgreSQL

Добавляем роль
```
sudo -u postgres createuser --pwprompt zabbix
```

Добавляем базу
```
sudo -u postgres createdb -O zabbix zabbix
```

Импортируем схему
```
sudo zcat /usr/share/doc/zabbix-server-pgsql*/create.sql.gz | sudo -u zabbix psql zabbix
```

## Настройте базу данных для Zabbix
В файле
```
sudo nano /etc/zabbix/zabbix_server.conf
```
Изменить строку
```
DBPassword=password
```

## Настройте PHP для веб-интерфейса
В файле
```
sudo nano /etc/zabbix/apache.conf
```
Изменяем строку
```
php_value date.timezone Asia/Yekaterinburg
```

## Запустите процессы Zabbix сервера и агента
Запуск при загрузке ОС:
```
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```

## Настройте веб-интерфейс Zabbix

Откройте установленный веб-интерфейс Zabbix: http://[IP_ADRES]/zabbix
Выполните действия по этой инструкции: Установка веб-интерфейса Zabbix


### Настроить pg_hba.conf
Настройка доступа по сетям, в файле pg_hba.conf
```
sudo nano /etc/postgresql/10/main/pg_hba.conf
```
Пример: разрешить доступ локально из подсетей в режиме trust
```
# Database administrative login by Unix domain socket
local   all             postgres                                trust

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
```

### Настроить postgresql.conf
Настройка доступа по адресам, в файле postgresql.conf
```
sudo nano /etc/postgresql/10/main/postgresql.conf
```
Пример: Доступ со всех адресов
```
# - Connection Settings -
#listen_addresses = 'localhost'
listen_addresses = '*'
```

### Создать базу данных из скрипта
```
sudo zcat /usr/share/doc/zabbix-server-pgsql/create.sql.gz | psql -U zabbix zabbix
```

### Запуск процесса Zabbix сервера
Самое время запустить процесс Zabbix сервера и добавить его в автозагрузку:
```
sudo service zabbix-server start
sudo update-rc.d zabbix-server enable
```

```
sudo nano /etc/nginx/sites-available/default
```

### перезапустить nginx
```
nginx -s reload
```

### Настройка веб-интерфейса
Файл конфигурации nginx
```
sudo nano /etc/zabbix/nginx.conf
```
текст
```
listen          80;
```

Файл конфигурации php
```
sudo nano /etc/zabbix/php-fpm.conf
```
текст
```
php_value max_execution_time 300
php_value memory_limit 128M
php_value post_max_size 16M
php_value upload_max_filesize 2M
php_value max_input_time 300
php_value max_input_vars 10000
php_value always_populate_raw_post_data -1
php_value date.timezone Asia/Yekaterinburg
```

Перезапуск nginx
```
nginx -s reload
```
