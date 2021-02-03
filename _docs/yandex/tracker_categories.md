---
title: "Создание категории"
permalink: /docs/yandex/tracker_categories/
toc: true
---

Категории позволяю группировать поля в задаче.
Возможности создать категории из интерфейса нет
а так-же нет в описании API.

Важно! Категорию переименовать нельзя.

Создание категории используя HTTP-запрос с методом POST.
Параметры тела в формате JSON.
```
POST /v2/fields/categories?expand=fields HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-ID: <orgid>
Content-Type: application/json

{
"name": {
"en": "<en>",
"ru": "<ru>"
},
"description": "<description>",
"order": <order>
}
```
где
<token> - ваш токен
<orgid> - номер организации
<en> - название категории на английском
<ru> - название категории на русском
<description> - описание категории
<order> - порядок отображения

Получить категории
```
GET /v2/fields/categories HTTP/1.1
Host: api.tracker.yandex.net
Authorization: OAuth <token>
X-Org-Id: 920716
```
