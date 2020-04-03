---
title: "Публикация: Равертывание сайта документации"
date: 2020-04-03
categories:
  - Post Formats
tags:
  - gallery
  - Post Formats
  - tiled
---

Цель. Создасть Сайт содержащий раздел новости и руководство пользователя,
где редактирование странци выполняется с сипользование разметки markdown.

Для этого потребуется:
- регистрация на GitHub
- GitHub Desktop
- Atom

## Клонировать репоизторий
https://github.com/mmistakes/minimal-mistakes

## Настроить `_config.yml`

- Заполнить название
- Контактные данные
- Разместить логотип

```
# Тема
remote_theme             : "mmistakes/minimal-mistakes@4.19.1"
minimal_mistakes_skin    : "default" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"

# Site Settings
locale                   : "ru-RU"
title                    : "Название"
title_separator          : "-"
subtitle                 : ""
name                     : &name "" # &name is a YAML anchor which can be *referenced later
```

### Установить русский язык

- в `_config.yml` установить `locale: "ru-RU"`
- скопировать `_data\ui-text.yml`

### Удалить лишние разделы

portfolio, pets, recipes в

- `_config.yml` в разделе `collections:`, `Defaults:`
- `_data\navigation.yml` в `main`
- `_pages\portfolio-archive.md`
- `docs\_pages\recipes-archive.md`
- `docs\_pages\pets.md`


### Удалить comments

- `_data\comments`

### Удалить posts

- при удалении `2010-09-09-post-gallery` удалить ссылку в _docs\14-helpers.md
- Кроме `_posts\9999-12-31-post-future-date.md`



7343.58 ₽в месяц
10247.63 ₽в месяц
