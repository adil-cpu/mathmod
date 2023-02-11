---
## Front matter
lang: ru-RU
title: Презентация лабораторной работы №1
subtitle: Математическое моделирование
author:
  - Мухамедияр А.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 февраля 2023

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Мухамедияр Адиль
  * группа НКНбд-01-20
  * Факультет физико-математических и естественных наук
  * Российский университет дружбы народов
  * <https://github.com/adil-cpu>



## Цели и задачи

- Создать шаблон репозитория для выполнения лабораторных работ
- Освежить и дополнить навыки владения git, Markdown, командной строкой

## Материалы и методы

- Процессор `pandoc` для входного формата Markdown
- Результирующие форматы
	- `pdf`
	- `html`
- Автоматизация процесса создания: `Makefile`
- Технология git

# Выполнение лабораторной работы

1. Для начало мы создали удаленный репозиторий из шаблона

- Репозиторий: <https://github.com/adil-cpu/mathmod>

![](../report/img/git.png){#fig:001 width=70%}

2. Дальше мы создаем каталог и клонируем удаленный репозиторий используя командную строку
![](../report/img/pr1.png){#fig:002 width=70%}
![](../report/img/pr2.png){#fig:003 width=70%}

## Настройка необходимой иерархии

- Используя командную строку и команду `make` настроили необходимую иерархию

![](../report/img/pr3.png){#fig:004 width=70%}


## Написание отчета

- Используя Markdown и vim написали отчет о проделанной работе

![](../report/img/pr4.png){#fig:005 width=70%}

# Результаты

Настроили все необходимые каталоги и репозитории для успешного выполнения последующих лабораторных работ.
