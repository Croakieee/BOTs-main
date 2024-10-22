# Торговый бот ([english version](README.md))
[![Торговля с ботом](trading.png)](trading.png)

## Предварительные требования
* Linux, macOS, Windows
* [Git](https://git-scm.com/downloads)
* [.NET SDK](https://dotnet.microsoft.com/download)

## Установка
```
cd Bot/csharp/TraderBot
dotnet restore
```

## Подготовка

Для работы бота вам необходимо получить токен. Вы можете сделать это здесь: https://www.tinkoff.ru/invest/settings.

Токен должен быть установлен в файле `appsettings.json` в ключе `InvestApiSettings`/`AccessToken`. Также установите `TradingSettings`/`AccountIndex` в индекс брокерского счёта, к которому у токена есть доступ. Список брокерских счетов с их индексами бот пишет в консоль при запуске.

## Запуск
```sh
dotnet run
```

Для остановки бота нажмите Ctrl+C.

## Описание

В настоящее время бот имеет следующие возможности:
* Продажа по лучшей цене или по цене покупки;
* Покупка по лучшей цене;
* Перемещение ордеров на покупку и продажу.

После подачи заявки бот будет ждать ее завершения. Затем он подаст следующую заявку:
* После исполнения заявки на покупку будет продавать по лучшей цене или цене покупки;
* После исполнения заявки на продажу будет покупать по лучшей цене.

Вы можете настроить торговлю ETF/наличными ботом в ключах `TradingSettings`/`EtfTicker` и `TradingSettings`/`CashCurrency` в файле `appsettings.json`.

Чтобы бот продавал только с прибылью (+1 пункт или, например, +0.01), установите в `TradingSettings`/`AllowSamePriceSell` значение `false`. Это может ограничить возможность бота следовать за ценой (особенно вниз).

## Стратегия

Этот бот реализует простую стратегию скальпинга.
Он выполняет постоянный цикл покупок и продаж иногда получая прибыль размером в 1 пункт за итерацию покупки и продажи.
Работает наилучшим образом на растущем рынке и с наименее волотильным активом.

Гипотеза:
```
Используя эту стратегию, можно получить больше прибыли,
чем при покупке и удержании,
из-за эффекта сложного процентов
на каждой итерации покупки и продажи с доходом.
```

## Дорожная карта
- [x] Сделать бота, который может подавать заявки на покупку и продажу по лучшей цене за один запуск
- [x] Сделать чтобы бот работал без перезапусков
- [x] Поддержка перемещения заявки на покупку вверх
- [x] Опциональная поддержка перемещения ордера на продажу вниз (до уровня, равного цене покупки)
- [ ] Добавить режим короткой торговли
- [ ] Оптимизировать количество запросов к API
- [ ] Хранить все данные в ассоциативной базе данных ([Deep](https://github.com/deep-foundation) или [хранилище связей-дуплетов](https://github.com/linksplatform/Data.Doublets))
