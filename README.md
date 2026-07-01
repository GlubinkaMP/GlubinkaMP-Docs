# 📚 Glubinka MP Documentation

Официальная документация для разработчиков ресурсов платформы **Glubinka MP**.

Этот репозиторий содержит руководства, описания API, примеры и справочные материалы по разработке серверных ресурсов.

---

## 🌐 Открыть документацию

Если для репозитория уже включён GitHub Pages, документация будет доступна по ссылке:

**https://glubinkamp.github.io/GlubinkaMP-Docs/**

> Если сайт ещё не открылся — это нормально.  
> GitHub Pages может публиковать изменения несколько минут.

---

## 🚀 Быстрый старт

Если ты только начинаешь, открывай разделы в таком порядке:

1. [Главная страница документации](./docs/index.md)
2. [Events](./docs/Events.md)
3. [Commands](./docs/Commands.md)
4. [Players](./docs/Players.md)
5. [Vehicles](./docs/Vehicles.md)
6. [World](./docs/World.md)
7. [CEF UI](./docs/CEF_UI.md)
8. [FAQ](./docs/FAQ.md)

---

## 📖 Что находится в этом репозитории

- **Events** — все серверные события
- **Commands** — создание команд
- **Players** — работа с игроками
- **Vehicles** — работа с транспортом
- **World** — управление погодой, временем и игровым миром
- **CEF UI** — создание интерфейсов
- **FAQ** — ответы на частые вопросы

---

## 🧩 Для кого эта документация

Этот репозиторий нужен для:

- разработчиков игровых ресурсов
- владельцев серверов, которые настраивают скрипты
- людей, которые хотят писать команды, игровые режимы, интерфейсы и системы

---

## ⚠️ Важно про namespace

Несмотря на то, что платформа называется **Glubinka MP**, в коде ресурсов используются внутренние пространства имён, оставленные для совместимости:

```csharp
using GTANetworkServer;
using GTANetworkShared;
```
Это нормально и ожидаемо.

---

## 📂 Структура документации
```
docs/
├── index.md
├── Events.md
├── Commands.md
├── Players.md
├── Vehicles.md
├── World.md
├── CEF_UI.md
└── FAQ.md
```
---

## 🔗 Полезные ссылки

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)

---

© Glubinka MP
Glubinka MP — мультиплеерная платформа для GTA V.

Использование платформы, SDK и связанных материалов регулируется лицензией и правилами проекта Glubinka MP.
