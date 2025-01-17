---
id: quick-setup
title: Быстрая настройка
slug: /how-to/build-badge-app/quick-setup
sidebar_label: Быстрая настройка
sidebar_position: 1
description: Как создать приложение для бейджей - Быстрая настройка
---

Это руководство научит вас, как создать социальное приложение, в котором пользователи могут создавать on-chain бейджи, которые могут быть привязаны к мероприятиям. Как только пользователи настроят свои профили, они смогут создавать бейджи для мероприятия, а участники смогут собирать их, посещая это мероприятие.

Это базовый пример преследует единственную цель - рассмотреть основные функции и подчеркнуть, насколько легко их реализовать. Позже вы сможете экстраполировать их и проявить творческий подход к своему проекту, чтобы создать различные варианты использования, которые действительно выделят ваше приложение.

## Содержание

Руководство по созданию приложения для бейджей включает следующие разделы:

1. [Создайте профиль](/how-to/build-badge-app/create-a-profile)
2. [Аутентификация](/how-to/build-badge-app/authentication)
3. [Создайте бейдж](/how-to/build-badge-app/create-a-badge)
4. [Соберите бейдж](/how-to/build-badge-app/collect-a-badge)

## Требования

Приложение, которое вы собираетесь создать, использует [Next.js](https://nextjs.org/). Убедитесь, что вы установили [Node.js](https://nodejs.org/en/download/) на вашем компьютере и расширение [MetaMask](https://metamask.io/) в вашем браузере Chrome.

## Установка

Клонируйте репозиторий [https://github.com/cyberconnecthq/cc-badge-app.git](https://github.com/cyberconnecthq/cc-badge-app.git) и запустите следующую команду в вашем терминале, чтобы установить все пакеты, необходимые для запуска сервера разработки: `npm install` или `yarn install`.

## Локальная разработка

Чтобы запустить локальный сервер разработки, выполните команду `npm run dev` или `yarn dev` и откройте в окне браузера http://localhost:3000. Большинство изменений отражаются в реальном времени без необходимости перезагрузки сервера.

## Live демонстрация

Это ссылка на действующую версию приложения, которое вы собираетесь создать: [https://cc-badge-app.vercel.app/](https://cc-badge-app.vercel.app/)

Давайте начнем с создания приложения, в котором пользователи могут выдавать и собирать бейджи!
