---
title: "install-postgresql"
permalink: /docs/ubuntu/install-postgresql/
toc: true
---

# Установка (Install) PostgreSQL

## Подготовка
```
sudo apt-get update   
sudo apt-get install mc
sudo apt-get install nano
sudo mc
sudo timedatectl set-timezone Asia/Yekaterinburg
```

## Установка locale

[Инструкция help.ubuntu.ru](https://help.ubuntu.ru/wiki/%D1%80%D1%83%D1%81%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F_ubuntu)

### Просмотр установленных локалей
```
locale -a | grep ru
```

### Установка языкового пакета language-pack-ru
```
sudo apt-get install language-pack-ru
```

### Установка локалей
Через консоль (рекомендуемый вариант)
```
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
```
или через графический интерфейс, где нужно выбрать - ru_RU.UTF-8, ru_RU.CP1251
```
sudo dpkg-reconfigure locales
sudo apt-get install --reinstall locales
```

### Обновление локалей
```
sudo update-locale
sudo update-locale LANG=ru_RU.UTF-8
```    

### Проверка локали по умолчанию
запустить
```
sudo nano /etc/default/locale
```
должен быть текст
```
LANGUAGE=ru_RU:ru
LANG=ru_RU.UTF-8
```

### Проверка установки локали
```
locale -a | grep ru
```

### Перезагрузка
```
sudo reboot
```

## Создать файл с репозиторием
``
sudo nano /etc/apt/sources.list.d/pgdg.list
``
и добавить в нем строку
``
deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
``

## Установка PostgreSQL

[postgresql.org - Linux downloads (Ubuntu)](https://www.postgresql.org/download/linux/ubuntu/)

### Импортировать репозиторий
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
```

### Установить PostgreSQL
- версия 11
```
apt-get install postgresql-11
```
- версия 10
```
apt-get install postgresql-10
```
- версия 9.6
```
apt-get install postgresql-9.6
```

### Проверить работу кластера
- Список установленных кластеров
```
pg_lsclusters
```
- Запуск кластера
```
- sudo service postgresql start
```
- Остановка кластера
```
sudo service postgresql stop
```

### Настроить pg_hba.conf
Настройка доступа по сетям, в файле pg_hba.conf
- версия 11
```
sudo nano /etc/postgresql/11/main/pg_hba.conf
```
- версия 10
```
sudo nano /etc/postgresql/10/main/pg_hba.conf
```
- версия 9.6
```
sudo nano /etc/postgresql/9.6/main/pg_hba.conf
```
Пример: разрешить доступ локально из подсетей в режиме trust
```
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
host    all             all             10.128.0.0/24           trust
host    all             all             10.129.0.0/24           trust
host    all             all             10.130.0.0/24           trust
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

## Установка пароля postgres
Зайти в psql
```
sudo -u postgres psql template1
```
Выполнить скрипт изменяющий пароль.
Важно! [Пароль] заменить на нужный
```
ALTER USER postgres with encrypted password '<PASSWORD>';
```
Выйти
```
\q
```
Перезапустить PostgreSQL
```
sudo /etc/init.d/postgresql restart
```
