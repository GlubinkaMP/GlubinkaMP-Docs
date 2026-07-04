---
title: CEF UI
nav_order: 16
---

# CEF UI

Документация по созданию пользовательских интерфейсов через CEF в **Glubinka MP**.

CEF (Chromium Embedded Framework) позволяет создавать игровые интерфейсы с помощью HTML, CSS и JavaScript. Интерфейсы отображаются поверх игры на стороне клиента.

---

## Что такое CEF в Glubinka MP

CEF — это встроенный браузер внутри игры. Ты создаёшь обычные HTML-страницы с CSS и JavaScript, а клиент Glubinka MP отображает их поверх экрана.

Это используется для:

- меню входа / регистрации
- HUD (здоровье, деньги, информация)
- инвентарь
- диалоги с NPC
- карта, GPS
- любые кастомные интерфейсы

---

## Версия CEF

```
CEF 3.2987 / Chromium 57 (2017)
```

Это старая версия Chromium. Некоторые современные возможности могут не работать.

---

## Что поддерживается

- HTML5
- CSS3 (включая Flexbox)
- JavaScript (ES6 базовый)
- Локальные файлы (HTML, CSS, JS, изображения)
- `XMLHttpRequest`

---

## Чего нет

- CSS Grid (частично)
- ES2017+ (`async/await` могут не работать)
- Современные Web API
- `Fetch API` — используй `XMLHttpRequest`
- Внешние CDN (нет доступа в интернет)

---

## Рекомендуемый стек

- HTML + CSS + vanilla JavaScript
- Flexbox для вёрстки
- Локальные файлы (всё в папке ресурса)
- Минимум зависимостей
- Никаких фреймворков (React, Vue и т.п. могут не работать)

---

## Структура файлов

Файлы интерфейса лежат в папке ресурса:

```
resources/
└── myresource/
    ├── meta.xml
    ├── main.cs
    └── client/
        ├── index.html
        ├── style.css
        └── script.js
```

В `meta.xml` укажи клиентские файлы:

```xml
<meta>
    <info name="MyResource" version="1.0" author="Me" />
    <script src="main.cs" type="server" lang="csharp" />
    <file src="client/index.html" />
    <file src="client/style.css" />
    <file src="client/script.js" />
</meta>
```

---

## Создание браузера на клиенте

CEF-браузер создаётся на **клиентской стороне**.

### Клиентский скрипт (JavaScript или C#)

```csharp
var browser = API.createCefBrowser(1280, 720);
API.setCefBrowserPosition(browser, 0, 0);
API.loadPageCefBrowser(browser, "client/index.html");
API.showCursor(true);
API.setCefBrowserHeadless(browser, false);
```

### Закрыть браузер

```csharp
API.destroyCefBrowser(browser);
API.showCursor(false);
```

---

## Пример HTML-страницы

### `client/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Добро пожаловать</h1>
        <p>Glubinka MP Server</p>
        <button id="closeBtn">Закрыть</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

### `client/style.css`

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    font-family: Arial, sans-serif;
    color: white;
}

.container {
    background: rgba(0, 0, 0, 0.8);
    padding: 40px;
    border-radius: 10px;
    text-align: center;
}

h1 {
    margin-bottom: 10px;
    color: #4CAF50;
}

button {
    margin-top: 20px;
    padding: 10px 30px;
    background: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

button:hover {
    background: #45a049;
}
```

### `client/script.js`

```javascript
document.getElementById("closeBtn").addEventListener("click", function() {
    resourceCall("closeUI");
});
```

---

## Взаимодействие CEF ↔ Сервер

### Из CEF → Клиент

В JavaScript вызывай:

```javascript
resourceCall("eventName", arg1, arg2);
```

### Из Клиента → CEF

```csharp
API.triggerCefBrowserEvent(browser, "eventName", arg1, arg2);
```

### Из Клиента → Сервер

```csharp
API.triggerServerEvent("eventName", arg1, arg2);
```

### Из Сервера → Клиент

```csharp
API.triggerClientEvent(client, "eventName", arg1, arg2);
```

### Схема взаимодействия

```
CEF (JavaScript)
    ↓ resourceCall
Клиент (C# / JS)
    ↓ triggerServerEvent
Сервер (C#)
    ↓ triggerClientEvent
Клиент (C# / JS)
    ↓ triggerCefBrowserEvent
CEF (JavaScript)
```

---

## Пример — экран входа

### Серверная часть

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class LoginResource : GlubinkaContext
{
    protected override void OnStart()
    {
        API.onPlayerConnected += OnPlayerConnected;
    }

    protected override void OnStop()
    {
        API.onPlayerConnected -= OnPlayerConnected;
    }

    private void OnPlayerConnected(Client client)
    {
        var player = WrapPlayer(client);
        player.Freeze();

        // Показать экран входа на клиенте
        API.triggerClientEvent(client, "showLoginScreen");
    }
}
```

### Клиентская часть (C#)

```csharp
Browser loginBrowser = null;

API.onClientEventTrigger += (eventName, args) =>
{
    if (eventName == "showLoginScreen")
    {
        loginBrowser = API.createCefBrowser(1280, 720);
        API.setCefBrowserPosition(loginBrowser, 0, 0);
        API.loadPageCefBrowser(loginBrowser, "client/login.html");
        API.showCursor(true);
    }
};

// Обработка события из CEF
API.onCefBrowserEvent += (browser, eventName, args) =>
{
    if (eventName == "loginSubmit")
    {
        string username = args[0].ToString();
        string password = args[1].ToString();

        API.triggerServerEvent("onLoginAttempt", username, password);

        API.destroyCefBrowser(loginBrowser);
        API.showCursor(false);
    }
};
```

### `client/login.html`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="login.css">
</head>
<body>
    <div class="login-box">
        <h1>Авторизация</h1>
        <input type="text" id="username" placeholder="Логин">
        <input type="password" id="password" placeholder="Пароль">
        <button id="loginBtn">Войти</button>
    </div>
    <script>
        document.getElementById("loginBtn").addEventListener("click", function() {
            var username = document.getElementById("username").value;
            var password = document.getElementById("password").value;
            resourceCall("loginSubmit", username, password);
        });
    </script>
</body>
</html>
```

---

## Советы по разработке UI

### 1. Используй Flexbox

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

### 2. Делай фон полупрозрачным

```css
body {
    background: rgba(0, 0, 0, 0.5);
}
```

### 3. Не используй внешние шрифты из CDN

Скачай шрифт и положи в папку ресурса:

```css
@font-face {
    font-family: 'MyFont';
    src: url('myfont.ttf');
}
```

### 4. Тестируй интерфейс в обычном браузере

Открой `index.html` в Chrome — так проще отладить вёрстку. Но помни, что CEF в Glubinka MP старее — некоторые вещи могут работать иначе.

### 5. Минимизируй количество файлов

Меньше файлов — быстрее загрузка. По возможности объединяй CSS и JS.

---

## Частые ошибки

### Ошибка 1 — используешь Fetch API

Неправильно:

```javascript
fetch("/api/data").then(r => r.json());
```

Правильно:

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api/data", true);
xhr.onload = function() {
    var data = JSON.parse(xhr.responseText);
};
xhr.send();
```

---

### Ошибка 2 — используешь внешний CDN

Неправильно:

```html
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>
```

Правильно — все файлы должны быть локальными:

```html
<script src="script.js"></script>
```

---

### Ошибка 3 — забыл указать файлы в meta.xml

Неправильно:

```xml
<meta>
    <script src="main.cs" type="server" lang="csharp" />
</meta>
```

Правильно — укажи HTML, CSS и JS файлы:

```xml
<meta>
    <script src="main.cs" type="server" lang="csharp" />
    <file src="client/index.html" />
    <file src="client/style.css" />
    <file src="client/script.js" />
</meta>
```

---

### Ошибка 4 — не показал курсор

Неправильно — браузер видно, но нельзя кликать:

```csharp
var browser = API.createCefBrowser(1280, 720);
API.loadPageCefBrowser(browser, "client/index.html");
```

Правильно:

```csharp
var browser = API.createCefBrowser(1280, 720);
API.loadPageCefBrowser(browser, "client/index.html");
API.showCursor(true);
```

---

### Ошибка 5 — не уничтожил браузер при закрытии

Неправильно — браузер продолжает работать:

```javascript
document.getElementById("closeBtn").addEventListener("click", function() {
    // ничего не делает
});
```

Правильно:

```javascript
document.getElementById("closeBtn").addEventListener("click", function() {
    resourceCall("closeUI");
});
```

И на клиенте:

```csharp
API.destroyCefBrowser(browser);
API.showCursor(false);
```

---

## См. также

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
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
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
