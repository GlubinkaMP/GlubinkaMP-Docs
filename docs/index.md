# 📘 Glubinka MP Documentation

Добро пожаловать в официальную документацию для разработчиков ресурсов платформы **Glubinka MP**.

Здесь собраны основные разделы по серверному API, событиям, командам, работе с игроками, транспортом, игровым миром и интерфейсами CEF.

---

## 🧭 С чего начать

Если ты открываешь документацию впервые, читай разделы в таком порядке:

1. [Events](./Events.md)
2. [Commands](./Commands.md)
3. [Players](./Players.md)
4. [Vehicles](./Vehicles.md)
5. [World](./World.md)
6. [CEF UI](./CEF_UI.md)
7. [FAQ](./FAQ.md)

---

## 📚 Разделы документации

### 📡 API и события
- [Events](./Events.md) — список серверных событий, сигнатуры и примеры использования

### ⌨️ Команды
- [Commands](./Commands.md) — создание серверных команд, параметры и примеры

### 👤 Игроки
- [Players](./Players.md) — здоровье, броня, телепорт, оружие, скин, сообщения

### 🚗 Транспорт
- [Vehicles](./Vehicles.md) — создание, удаление, ремонт, цвета, модификации, посадка игроков

### 🌍 Мир
- [World](./World.md) — погода, время, глобальные параметры мира

### 🖥️ Интерфейсы
- [CEF UI](./CEF_UI.md) — создание HTML/CSS/JS интерфейсов через встроенный CEF

### ❓ Частые вопросы
- [FAQ](./FAQ.md) — типовые проблемы и быстрые ответы

---

## 🧩 Что нужно для разработки

Обычно для написания ресурса тебе понадобятся:

- сервер Glubinka MP
- публичный SDK Glubinka MP
- Visual Studio 2022
- .NET Framework 4.7.2
- базовые знания C#

---

## ⚠️ Важно

### 1. В коде используются внутренние namespace
Для ресурсов используются пространства имён:

```csharp
using GTANetworkServer;
using GTANetworkShared;
```

Это сделано для совместимости ядра и SDK.

### 2. Документация пишется для разработчиков ресурсов
Здесь описывается именно работа с API и созданием игровых ресурсов.
Исходный код ядра платформы в эту документацию не входит.

### 3. Некоторые разделы могут дополняться
Документация развивается постепенно. Если какой-то раздел ещё короткий — он будет расширяться.

🚀 Быстрый переход
- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)

## 🌐 Сайт документации
Если GitHub Pages уже включён, документация доступна по адресу:

**https://glubinkamp.github.io/GlubinkaMP-Docs/**

© Glubinka MP
Официальная документация платформы Glubinka MP.
