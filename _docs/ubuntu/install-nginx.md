---
title: "install-nginx"
permalink: /docs/install-nginx/
toc: true
---

# Установка Ubunt и подключение

	1. Установить сервер Ubunt lts
	    - login as: arctl
		- Passphrase for key "rsa-key-20190220": PerryMason
		- сгенерировать ssh с использование puttyGen
	2. Подключиться через putty
		- ssh путь до private key
	3. Обновить сервер
		sudo apt-get update    
	4. Перезагрузить
		sudo reboot
	6. Установить GNU Midnight Commander
		sudo apt-get install mc
		# Текстовый редактор
		sudo apt-get install nano
		# запустить
		sudo mc			
    7. Установить временную зону
		# Временные зоны
        # timedatectl list-timezones
		sudo timedatectl set-timezone Asia/Yekaterinburg
		# Проверка даты
		date
	5. Установка nginx
		sudo apt-get install nginx

# Настройка nginx
	1. В папкe /etc/nginx/conf.d/demo
       создать файлы конфигураций для сайтов, пример demo.arctl.ru.conf	   	
		server {
				server_name demo.arctl.ru www.demo.arctl.ru;
				location / {
						proxy_pass http://130.193.41.115:80;
						proxy_read_timeout 1200;
						proxy_send_timeout 1200;
						proxy_buffer_size 128k;
						proxy_buffers 4 256k;
						proxy_busy_buffers_size 256k;
						proxy_set_header Host $host;
						proxy_set_header X-Real-IP $remote_addr;
						proxy_set_header X-Forvarder-for $remote_addr;
						proxy_set_header X-Colibri-Version "1.1.1";
						proxy_hide_header X-AspNet-Version;
						client_max_body_size 0m;
				}
	2. Выполнить перезагрузка конфигурационного файла  
		nginx -s reload

# Сertbot
  - Automatically enable HTTPS on your website with EFF's Certbot, deploying Let's Encrypt certificates
  - https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx

	Install (установка)
	$ sudo apt-get update
	$ sudo apt-get install software-properties-common
	$ sudo add-apt-repository universe
	$ sudo add-apt-repository ppa:certbot/certbot
	$ sudo apt-get update
	$ sudo apt-get install certbot python-certbot-nginx

	Get Started (начало работы)
	$ sudo certbot --nginx

	Automating renewal (автоматическое продление)
	$ sudo certbot renew --dry-run

        Продление сетефикатов
        certbot -q renew

	# Тест настроек nginx
	service nginx configtest

# Настройка кэша
  -	https://ruhighload.com/%D0%9A%D1%8D%D1%88%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5+%D1%81+nginx

  - nginx.conf
	proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=all:30m max_size=1g;

        proxy_cache_path /var/cache/nginx/demo levels=1:2 keys_zone=demo:30m max_size=1g;
        proxy_cache_path /var/cache/nginx/dev  levels=1:2 keys_zone=dev:30m max_size=1g;
        proxy_cache_path /var/cache/nginx/tmp  levels=1:2 keys_zone=tmp:30m max_size=1g;


  -
	  server {
			listen 80;

			location / {
					proxy_pass http://127.0.0.1:81/;
					proxy_cache all;
					proxy_cache_valid any 1h;
					proxy_cache_bypass $cookie_session;
                                        proxy_cache_use_stale error timeout http_500 http_502 http_503 http_504;
					proxy_no_cache $cookie_session;

			}
	}

	 proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=all:30m max_size=1g;

# 400 Bad Request Request Header Or Cookie Too Large nginx/1.14.0 (Ubuntu)

sudo nano /etc/nginx/sites-available/example.com.conf

server {
    # ...
    large_client_header_buffers 4 16k;
    # ...
}

sudo systemctl reload nginx.service
