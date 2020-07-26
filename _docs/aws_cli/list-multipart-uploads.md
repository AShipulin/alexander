---
title: "Список загрузок"
permalink: /docs/aws_cli/list-multipart-uploads/
toc: true
---

## Получение спсиска загрузок

Описание
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket <bucket>
```
где <bucket> - имя вашего бакета


Пример. С выводом на экран
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket arctl-backup-files
```

Пример. С выводом в файл json
```
aws --endpoint-url=https://storage.yandexcloud.net s3api list-multipart-uploads --bucket arctl-backup-files > c:\tmp\list-multipart-uploads.json
```
