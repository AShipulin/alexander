# Установка (install) Zabbix Agent

[Zabbix Агенты](https://www.zabbix.com/ru/download_agents)

## Ubuntu

### Установите репозиторий Zabbix:
```
sudo wget https://repo.zabbix.com/zabbix/4.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.4-1+bionic_all.deb
sudo dpkg -i zabbix-release_4.4-1+bionic_all.deb
sudo apt update
```

### Установите Zabbix агента:
```
sudo apt -y install zabbix-agent
```

### Настройка Zabbix агента
в файле
```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
указать
```
Server=[IP-ADDRESS]
ServerActive=[IP-ADDRESS ZABBIX Сервера]
Hostname=[HOSTNAME]
```
где [IP-ADDRESS] - адрес zabbix сервера;
    [HOSTNAME]   - имя компьютера где запущен агент

Важно! [HOSTNAME] должен совпадать с [Имя узла сети] в zabbix

### Установить nmap
Утилита проверки сети и аудита безопасности
```
sudo apt update
sudo apt install nmap
```
Разрешение на запуск
```
sudo nano /etc/sudoers
```
и дописать
```
zabbix  ALL=(root) NOPASSWD: /usr/bin/nmap
```
или дописать автоматом (рекомендуемый вариант)
```
sudo echo "zabbix  ALL=(root) NOPASSWD: /usr/bin/nmap" >> /etc/sudoers
```

### Запуск
```
sudo systemctl enable zabbix-agent
sudo service zabbix-agent start
```
Важно! Первый раз нужно сделать перезапуск агента.
