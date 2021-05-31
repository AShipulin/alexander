---
title: "install-postgresql"
permalink: /docs/ubuntu/install-postgresql/
toc: true
---

# Установка (Install) PostgreSQL

## Подготовка

```
sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove
sudo timedatectl set-timezone Asia/Yekaterinburg
sudo apt-get install nano
sudo apt-get install mc
sudo mc
```

## Установка locale

[Инструкция help.ubuntu.ru](https://help.ubuntu.ru/wiki/%D1%80%D1%83%D1%81%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F_ubuntu)

### Просмотр установленных локалей

Проверка установленных

```
locale -a | grep ru
```

### Установка locale

Через консоль (рекомендуемый вариант)

```
sudo locale-gen ru_RU
sudo locale-gen ru_RU.UTF-8
```

или через графический интерфейс

```
sudo dpkg-reconfigure locales
```

### Переустановка locale

Выполнять в случае проблем с locale

```
sudo apt-get install --reinstall locales
```

### Обновление locale

Выполнять в случае проблем locale

```
sudo update-locale
sudo update-locale LANG=ru_RU.UTF-8
```

### Проверка locale по умолчанию

запустить

```
sudo nano /etc/default/locale
```

должен быть текст

```
LANGUAGE=ru_RU:ru
LANG=ru_RU.UTF-8
```

### Проверка установки locale

Выполнить перед установкой

```
locale -a | grep ru
```

### Установка языкового пакета language-pack-ru

Выполнять в случае проблем в приложении

```
sudo apt-get install language-pack-ru
```

### Перезагрузка

```
sudo reboot
```

# Установка PostgreSQL

[postgresql.org - Linux downloads (Ubuntu)](https://www.postgresql.org/download/linux/ubuntu/)

## Создать файл с репозиторием

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

## Импортировать ключ подписи

```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

## Установить последнюю версию

```
sudo apt-get update
sudo apt-get -y install postgresql
```

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

Список установленных кластеров

```
pg_lsclusters
```

## Запуск кластера

```
sudo service postgresql start
```

## Остановка кластера

```
sudo service postgresql stop
```

## Настроить pg_hba.conf

Настройка доступа по сетям, в файле `pg_hba.conf`

- версия 12
```
sudo nano /etc/postgresql/12/main/pg_hba.conf
```

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

# DataLens
host    all             all             178.154.242.176/28      trust
host    all             all             178.154.242.192/28      trust
host    all             all             178.154.242.208/28      trust
host    all             all             178.154.242.128/28      trust
host    all             all             178.154.242.144/28      trust
host    all             all             178.154.242.160/28      trust
```

## Настроить postgresql.conf

Настройка доступа по адресам, в файле `postgresql.conf`

- версия 12
```
sudo nano /etc/postgresql/12/main/postgresql.conf
```

- версия 11
```
sudo nano /etc/postgresql/11/main/postgresql.conf
```

- версия 10
```
sudo nano /etc/postgresql/10/main/postgresql.conf
```

Пример: Доступ со всех адресов
```
# - Connection Settings -
listen_addresses = '*'
```

Пример: Доступ только локально
```
# - Connection Settings -
listen_addresses = 'localhost'
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

```
\q
```

Перезапустить PostgreSQL
```
sudo /etc/init.d/postgresql restart
```

# Проверка версии Postgres

Версия psql
```
sudo psql --version
```

Установленные пакеты
```
dpkg -l | grep postgres
```

# Удаление Postgres

## 1. Список установленных пакетов
```
dpkg -l | grep postgres
```

## 2. Удаление пакетов
```
sudo apt-get --purge remove <НазваниеПакета>
```

## 3. Проверка удалённых пакетов
```
dpkg -l | grep postgres
```
