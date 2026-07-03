Это **главная страница документации** (`README.md` или `index.md`). Она нужна — это первое, что видит разработчик когда открывает документацию.

Но её нужно **переписать** — сейчас там проблемы:

- список разделов не включает новые SDK-файлы которые мы написали
- раздел "С чего начать" устарел — теперь есть `getting-started.md`
- раздел "Важно" про namespace немного устарел — теперь есть SDK
- ссылки ведут на старые разделы, новых нет

---

Переписываю целиком:

---

# 📘 Glubinka MP — Документация для разработчиков

Добро пожаловать в официальную документацию для разработчиков ресурсов платформы **Glubinka MP**.

Здесь собраны все разделы по работе с публичным SDK — игроки, транспорт, мир, чат, маркеры, колшейпы и многое другое.

---

## 🧭 С чего начать

Если ты открываешь документацию впервые — начни здесь:

1. [Getting Started](./getting-started.md) — как создать первый ресурс
2. [Player](./player.md) — работа с игроком
3. [Vehicle](./vehicle.md) — работа с транспортом
4. [World](./world.md) — погода, время, данные мира
5. [Chat](./chat.md) — сообщения и уведомления
6. [Events](./Events.md) — серверные события
7. [Commands](./Commands.md) — создание команд
8. [FAQ](./FAQ.md) — частые вопросы

---

## 📚 Разделы документации

### 🚀 Начало работы
- [Getting Started](./getting-started.md) — первый ресурс, GlubinkaContext, логгер

### 👤 Игроки
- [Player](./player.md) — здоровье, броня, телепорт, оружие, одежда, скин, сообщения
- [Players](./players.md) — список игроков, поиск, подсчёт

### 🚗 Транспорт
- [Vehicle](./vehicle.md) — создание, двигатель, двери, колёса, окна, неон, цвета, моды

### 🌍 Мир
- [World](./world.md) — погода, время, IPL, world data, synced data, настройки сервера

### 💬 Чат
- [Chat](./chat.md) — сообщения всем и одному, уведомления, цвета
- [ChatColor](./chat.md#chatcolor--цвета-текста) — готовые цветовые коды

### 🗺️ Объекты мира
- [Blip](./blip.md) — метки на карте
- [Marker](./marker.md) — 3D-маркеры
- [TextLabel](./textlabel.md) — 3D-текстовые метки
- [ColShape](./colshape.md) — невидимые зоны, события входа и выхода

### 🛠️ Утилиты
- [Logger](./logger.md) — логирование в консоль сервера
- [Config](./config.md) — чтение настроек из файла

### 📖 Справочники
- [Enums](./enums.md) — все enum'ы SDK в одном месте

### 📡 API и события
- [Events](./Events.md) — серверные события, сигнатуры и примеры
- [Commands](./Commands.md) — создание серверных команд

### 🖥️ Интерфейсы
- [CEF UI](./CEF_UI.md) — HTML/CSS/JS интерфейсы через встроенный CEF

### ❓ Частые вопросы
- [FAQ](./FAQ.md) — типовые проблемы и быстрые ответы

---

## 🧩 Что нужно для разработки

- Сервер Glubinka MP
- Публичный SDK Glubinka MP
- Visual Studio 2022
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
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html)
- [Blip](https://glubinkamp.github.io/GlubinkaMP-Docs/Blip.html)
- [Marker](https://glubinkamp.github.io/GlubinkaMP-Docs/Marker.html)
- [TextLabel](https://glubinkamp.github.io/GlubinkaMP-Docs/TextLabel.html)
- [ColShape](https://glubinkamp.github.io/GlubinkaMP-Docs/ColShape.html)
- [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html)
- [Config](https://glubinkamp.github.io/GlubinkaMP-Docs/Config.html)
- [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)

---

## 🌐 Сайт документации

**https://glubinkamp.github.io/GlubinkaMP-Docs/**

---

*© Glubinka MP — Официальная документация платформы.*
