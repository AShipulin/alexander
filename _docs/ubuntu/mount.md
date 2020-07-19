---
title: "mount"
permalink: /docs/ubuntu/mount/
toc: true
---

# Mount - Добавляем (монтируем) новый диск
В Upuntu присоденяются новый диск а новая папка.
Ниже пример монтрования нового диска data.

## Создем папку, где будет находиться диск
`sudo mkdir /mnt/data`

## Получем список дисков
Cписка дисков
`sudo fdisk -l`
Спсиок дисков с UUID
`blkid`

## Форматируем диск
Диск будет отфрмаматирован с именем data
```
sudo mkfs -t ext4 -L data /dev/vdb
```
где `/dev/vdb` - диск найденый на предыдущем шаге

## Монтируем диск
`sudo mount /dev/vdb /mnt/data`

## Добавляем диск в автозагрузку

### Вариант 1. (Реекомендуемый)
```
echo "LABEL=data /mnt/data ext4 defaults 0 0" | sudo tee -a /etc/fstab
```
Проверяем добавление
`sudo nano /etc/fstab`

### Вариант 2.
Выполняем команду показывающую UUID дисков
`blkid`
Запоимнаем UUID нужного диска, заменяем в скрипте и выполняем
```
echo <UUID> /mnt/data ext4 defaults 0 0" | sudo tee -a /etc/fstab
```
Проверяем добавление
`sudo nano /etc/fstab`
