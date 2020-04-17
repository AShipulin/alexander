# Установка (Install) Youtrack

## Подготовка
```
sudo apt-get update    
sudo apt-get install mc
sudo apt-get install nano
sudo mc
sudo timedatectl set-timezone Asia/Yekaterinburg
```

## Установка java
```
sudo apt-get install java
```
Проверка версии
```
java -version
```

## Установка Java Runtime Environment
```
apt install default-jre
```

## Установка пакета rng-tools
```
sudo apt-get install rng-tools
```

## Установка Youtrack
[Инструкция на английском на сайте jetbrains.com] https://www.jetbrains.com/help/youtrack/standalone/Install-YouTrack-JAR-as-Service-Linux.html

### Добавить пользователя
```
adduser youtrack --disabled-password
wget -O /home/youtrack/youtrack.jar https://download.jetbrains.com/charisma/youtrack-2019.1.52973.jar
chown youtrack:youtrack /home/youtrack/youtrack.jar
```

### Создать файл
```
/etc/systemd/system/youtrack.service
```
и добавить содержимое
```
[Unit]
Description=Youtrack
Requires=network.target
After=syslog.target network.target

[Service]
Type=simple
WorkingDirectory=/home/youtrack
ExecStart=/usr/bin/java -jar /home/youtrack/youtrack.jar --J-Xmx1G 8080
User=youtrack

[Install]
WantedBy=default.target

# Настроить службу
systemctl daemon-reload
systemctl enable youtrack.service
service youtrack start
```

### Перезагрузить машину
sudo reboot

## Восстановление из backup
[Инструкция на английском на сайте jetbrains.com](https://www.jetbrains.com/help/youtrack/standalone/Restore-JAR-Installation.html)

### Установить YouTrack заново как указано выше (обязательно перезагрузить)
```
sudo reboot
```

### Скачать backup youtrack.tar.gz
в папку /home/youtrack/
```
wget -O /home/youtrack/youtrack.tar.gz https://www.dropbox.com/s/7m7wkhi81ez2byv/2019-07-02-22-50-33.tar.gz?dl=0
```

### Запустить настройку и выбрать режим update
```
127.0.0.1:8080
```
-----------------
# Запуск youtrack вручную
# java -jar youtrack.jar

# Запустить и начать настройку
http://127.0.0.1:8080
