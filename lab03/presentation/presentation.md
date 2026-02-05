---
## Front matter
lang: ru-RU
title: Лабораторная работа №3
subtitle: Настройка прав доступа
author:
  - Ришард Когенгар
date: 18 января 2026

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получение навыков настройки базовых и специальных прав доступа для групп пользователей в ОС типа Linux.

# Ход выполнения работы

## Подготовка каталогов

- Вход под `root`
- Созданы: `/data/main`, `/data/third`
- Проверены владелец/группа

![Подготовка каталогов](Screenshot_1.png){ width=82% }

## Права 770 и проверка пользователя

- На каталоги выставлено `770`
- `bob` в группе **main** создаёт файл в `/data/main`
- Доступ в `/data/third` для `bob` запрещён

![Проверка доступа bob](Screenshot_2.png){ width=82% }

## Поведение без sticky bit

- `alice` создала файлы в `/data/main`
- `bob` смог удалить чужие файлы (запись в каталог разрешена группе)

![Удаление чужих файлов без sticky bit](Screenshot_3.png){ width=82% }

## setgid + sticky bit на /data/main

- Включён **setgid**: новые файлы наследуют группу каталога
- Включён **sticky bit**: нельзя удалять чужие файлы

![Эффект sticky bit](Screenshot_4.png){ width=82% }

## ACL на каталоги

- Для `/data/main`: группе **third** выдано `r-x`
- Для `/data/third`: группе **main** выдано `r-x`
- Проверка через `getfacl`

![ACL на каталоги](Screenshot_5.png){ width=82% }

## Проблема: ACL не наследуется

- Создан `newfile1`
- ACL каталога не перешёл на новый файл
- Нужны **default ACL**

![Отсутствие наследования](Screenshot_6.png){ width=82% }

## Default ACL: наследование включено

- Заданы **default ACL** для каталогов
- Создан `newfile2`
- Новый файл получил нужные ACL автоматически

![Наследование default ACL](Screenshot_7.png){ width=82% }

## Проверка доступа пользователем third

- Вход под `carol` (группа **third**)
- Операции доступа/записи зависят от ACL файла и прав на каталог
- Удаление чужих файлов ограничено (sticky bit)

![Проверка carol](Screenshot_8.png){ width=82% }

# Выводы

## Итог

- Настроены базовые права `chmod` и разграничение по группам
- Проверены **setgid** (наследование группы) и **sticky bit** (защита от удаления чужих файлов)
- Настроены **ACL** и **default ACL** для наследования прав на новые файлы
