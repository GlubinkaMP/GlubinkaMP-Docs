---
title: Главная
nav_order: 0
---

# 📘 Glubinka MP — Документация для разработчиков

Добро пожаловать в официальную документацию для разработчиков ресурсов платформы **Glubinka MP**.

Здесь собраны все разделы по работе с публичным SDK — игроки, транспорт, мир, чат, маркеры, колшейпы и многое другое.

---

## 🧭 С чего начать

Если ты открываешь документацию впервые — начни здесь:

1. [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html) — как создать первый ресурс
2. [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html) — работа с игроком
3. [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html) — работа со списком игроков
4. [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicle.html) — работа с транспортом
5. [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html) — погода, время, данные мира
6. [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html) — сообщения и уведомления
7. [Blip](https://glubinkamp.github.io/GlubinkaMP-Docs/Blip.html) — метки на карте
8. [Marker](https://glubinkamp.github.io/GlubinkaMP-Docs/Marker.html) — 3D-маркеры
9. [TextLabel](https://glubinkamp.github.io/GlubinkaMP-Docs/TextLabel.html) — 3D-текстовые метки
10. [СolShape](https://glubinkamp.github.io/GlubinkaMP-Docs/СolShape.html) — невидимые зоны
11. [Config](https://glubinkamp.github.io/GlubinkaMP-Docs/Config.html) — настройки ресурса
12. [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html) — логирование
13. [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html) — справочник enum'ов
14. [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html) — серверные события
15. [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html) — создание команд
16. [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html) — интерфейсы
17. [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html) — частые вопросы

---

## 📚 Разделы документации

### 🚀 Начало работы
- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html) — первый ресурс, GlubinkaContext, логгер

### 👤 Игроки
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html) — здоровье, броня, телепорт, оружие, одежда, скин, сообщения
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html) — список игроков, поиск, подсчёт

### 🚗 Транспорт
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicle.html) — создание, двигатель, двери, колёса, окна, неон, цвета, моды

### 🌍 Мир
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html) — погода, время, IPL, world data, synced data, настройки сервера

### 💬 Чат
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html) — сообщения всем и одному, уведомления, цвета

### 🗺️ Объекты мира
- [Blip](https://glubinkamp.github.io/GlubinkaMP-Docs/Blip.html) — метки на карте
- [Marker](https://glubinkamp.github.io/GlubinkaMP-Docs/Marker.html) — 3D-маркеры
- [TextLabel](https://glubinkamp.github.io/GlubinkaMP-Docs/TextLabel.html) — 3D-текстовые метки
- [СolShape](https://glubinkamp.github.io/GlubinkaMP-Docs/СolShape.html) — невидимые зоны, события входа и выхода

### 🛠️ Утилиты
- [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html) — логирование в консоль сервера
- [Config](https://glubinkamp.github.io/GlubinkaMP-Docs/Config.html) — чтение настроек из файла

### 📖 Справочники
- [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html) — все enum'ы SDK в одном месте

### 📡 API и события
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html) — серверные события, сигнатуры и примеры
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html) — создание серверных команд

### 🖥️ Интерфейсы
- [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html) — HTML/CSS/JS интерфейсы через встроенный CEF

### ❓ Частые вопросы
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html) — типовые проблемы и быстрые ответы

---

## 🧩 Что нужно для разработки

- [Сервер Glubinka MP](https://github.com/GlubinkaMP/GlubinkaMP-ServerKit/releases)
- [Публичный SDK Glubinka MP](https://github.com/GlubinkaMP/GlubinkaMP-SDK/releases) 
- [Visual Studio 2022](https://visualstudio.microsoft.com/ru/downloads/)
- .NET Framework 4.7.2
- Базовые знания C#

---

## ⚠️ Важно

### 1. Ресурсы пишутся через публичный SDK

Наследуй свой ресурс от `GlubinkaContext`:

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Log("Ресурс запущен!");
    }
}
```

### 2. Документация — для разработчиков ресурсов

Здесь описывается работа с публичным SDK и создание игровых ресурсов. Исходный код ядра платформы в эту документацию не входит.

### 3. Документация развивается

Некоторые разделы могут дополняться по мере развития платформы.

---

## 🚀 Быстрый переход

- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicle.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html)
- [Blip](https://glubinkamp.github.io/GlubinkaMP-Docs/Blip.html)
- [Marker](https://glubinkamp.github.io/GlubinkaMP-Docs/Marker.html)
- [TextLabel](https://glubinkamp.github.io/GlubinkaMP-Docs/TextLabel.html)
- [СolShape](https://glubinkamp.github.io/GlubinkaMP-Docs/СolShape.html)
- [Config](https://glubinkamp.github.io/GlubinkaMP-Docs/Config.html)
- [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html)
- [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)

---

*© Glubinka MP — Официальная документация платформы.*
