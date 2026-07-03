# 🖥️ CEF UI

Документация по созданию пользовательских интерфейсов через **CEF** (Chromium Embedded Framework) в **Glubinka MP**.

CEF позволяет рисовать HTML/CSS/JS интерфейсы поверх игры: HUD, меню, инвентарь, чат, скорборд и любые другие элементы.

---

## Что такое CEF

CEF — это встроенный браузер внутри игры.

Ты создаёшь обычные HTML-страницы, и они отображаются поверх экрана GTA V.

Игрок видит интерфейс и может взаимодействовать с ним мышкой и клавиатурой.

---

## Версия CEF в Glubinka MP

```text
CEF 3.2987 / Chromium 57 (2017)
```

Это старая версия. Она поддерживает:
```
- HTML5
- CSS3 (включая flexbox)
- JavaScript ES6 (базовый)
- Локальные файлы
```
Она **не поддерживает**:
```
- CSS Grid (частично)
- async/await (могут не работать)
- Fetch API (используй XMLHttpRequest)
- Современные Web API
- Внешние CDN (интернет-ресурсы)
```
---

## Рекомендуемый стек
```
| Что использовать | Что НЕ использовать |
|---|---|
| HTML + CSS + vanilla JS | React, Vue, Angular |
| Flexbox | CSS Grid |
| Локальные файлы | Внешние CDN |
| XMLHttpRequest | Fetch API |
| ES6 базовый | async/await |
| Простые шрифты | Google Fonts через CDN |
```
---

## Где лежат файлы CEF

Файлы интерфейса лежат внутри папки ресурса.

Пример структуры ресурса с CEF:

```text
resources\
└── myui\
    ├── meta.xml
    ├── myui.cs
    └── cef\
        ├── index.html
        ├── style.css
        └── script.js
```

---

## meta.xml для ресурса с CEF

```xml
<meta>
    <info name="MyUI" version="1.0" author="Dev" />
    <script src="myui.cs" type="server" lang="csharp" />
    <file src="cef/index.html" />
    <file src="cef/style.css" />
    <file src="cef/script.js" />
</meta>
```

> ⚠️ **Важно:** каждый файл CEF должен быть указан в `meta.xml` через тег `<file>`.  
> Иначе клиент не скачает его с сервера.

---

## Простой пример: HUD с текстом

### cef/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="hud">
        <div id="health">HP: 100</div>
        <div id="armor">Armor: 0</div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### cef/style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: transparent;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#hud {
    position: fixed;
    top: 20px;
    left: 20px;
    display: flex;
    flex-direction: column;
    gap: 5px;
}

#health {
    background: rgba(0, 0, 0, 0.6);
    color: #4CAF50;
    padding: 8px 16px;
    border-radius: 4px;
    font-size: 16px;
    font-weight: bold;
}

#armor {
    background: rgba(0, 0, 0, 0.6);
    color: #2196F3;
    padding: 8px 16px;
    border-radius: 4px;
    font-size: 16px;
    font-weight: bold;
}
```

---

### cef/script.js

```javascript
function updateHealth(value) {
    document.getElementById("health").textContent = "HP: " + value;
}

function updateArmor(value) {
    document.getElementById("armor").textContent = "Armor: " + value;
}
```

---

## Пример: меню с кнопками

### cef/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="menu" style="display: none;">
        <h1>Меню</h1>
        <button onclick="onButtonClick('heal')">Вылечиться</button>
        <button onclick="onButtonClick('armor')">Получить броню</button>
        <button onclick="onButtonClick('car')">Создать машину</button>
        <button onclick="onButtonClick('close')">Закрыть</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### cef/style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: transparent;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#menu {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, 0.85);
    padding: 30px;
    border-radius: 8px;
    min-width: 300px;
    text-align: center;
}

#menu h1 {
    color: #ffffff;
    font-size: 24px;
    margin-bottom: 20px;
}

#menu button {
    display: block;
    width: 100%;
    padding: 12px;
    margin-bottom: 10px;
    background: #4CAF50;
    color: #ffffff;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
}

#menu button:hover {
    background: #45a049;
}

#menu button:last-child {
    background: #f44336;
}

#menu button:last-child:hover {
    background: #d32f2f;
}
```

---

### cef/script.js

```javascript
function showMenu() {
    document.getElementById("menu").style.display = "block";
}

function hideMenu() {
    document.getElementById("menu").style.display = "none";
}

function onButtonClick(action) {
    resourceCall("menuAction", action);
}
```

---

## Пример: уведомление

### cef/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="notification" style="display: none;">
        <span id="notification-text"></span>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### cef/style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: transparent;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#notification {
    position: fixed;
    top: 20px;
    right: 20px;
    background: rgba(0, 0, 0, 0.8);
    color: #ffffff;
    padding: 12px 24px;
    border-radius: 6px;
    font-size: 16px;
    border-left: 4px solid #4CAF50;
    max-width: 400px;
}
```

---

### cef/script.js

```javascript
var notificationTimer = null;

function showNotification(text, duration) {
    var el = document.getElementById("notification");
    var textEl = document.getElementById("notification-text");

    textEl.textContent = text;
    el.style.display = "block";

    if (notificationTimer) {
        clearTimeout(notificationTimer);
    }

    notificationTimer = setTimeout(function() {
        el.style.display = "none";
    }, duration || 3000);
}
```

---

## Пример: простой скорборд

### cef/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="scoreboard" style="display: none;">
        <h2>Игроки на сервере</h2>
        <table>
            <thead>
                <tr>
                    <th>Имя</th>
                    <th>Пинг</th>
                </tr>
            </thead>
            <tbody id="player-list">
            </tbody>
        </table>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### cef/style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: transparent;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#scoreboard {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, 0.85);
    padding: 20px;
    border-radius: 8px;
    min-width: 400px;
}

#scoreboard h2 {
    color: #ffffff;
    text-align: center;
    margin-bottom: 15px;
    font-size: 20px;
}

#scoreboard table {
    width: 100%;
    border-collapse: collapse;
}

#scoreboard th {
    background: rgba(255, 255, 255, 0.1);
    color: #aaaaaa;
    padding: 8px 12px;
    text-align: left;
    font-size: 14px;
}

#scoreboard td {
    color: #ffffff;
    padding: 8px 12px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    font-size: 14px;
}
```

---

### cef/script.js

```javascript
function showScoreboard() {
    document.getElementById("scoreboard").style.display = "block";
}

function hideScoreboard() {
    document.getElementById("scoreboard").style.display = "none";
}

function updateScoreboard(playersJson) {
    var players = JSON.parse(playersJson);
    var tbody = document.getElementById("player-list");
    tbody.innerHTML = "";

    for (var i = 0; i < players.length; i++) {
        var row = document.createElement("tr");

        var nameCell = document.createElement("td");
        nameCell.textContent = players[i].name;

        var pingCell = document.createElement("td");
        pingCell.textContent = players[i].ping + " ms";

        row.appendChild(nameCell);
        row.appendChild(pingCell);
        tbody.appendChild(row);
    }
}
```

---

## Советы по вёрстке

### 1. Всегда ставь прозрачный фон

```css
body {
    background: transparent;
}
```

Иначе весь экран закроется белым или чёрным цветом.

---

### 2. Используй rgba для полупрозрачности

```css
background: rgba(0, 0, 0, 0.7);
```

Это чёрный фон с 70% непрозрачности.

---

### 3. Используй flexbox для расположения

```css
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
```

---

### 4. Используй position fixed для элементов поверх экрана

```css
#hud {
    position: fixed;
    top: 20px;
    left: 20px;
}
```

---

### 5. Скрывай overflow

```css
body {
    overflow: hidden;
}
```

Это убирает полосы прокрутки.

---

### 6. Не используй внешние шрифты через CDN

Не будет работать:

```html
<link href="https://fonts.googleapis.com/css2?family=Roboto" rel="stylesheet">
```

Вместо этого положи шрифт локально:

```text
cef/
├── fonts/
│   └── Roboto-Regular.ttf
```

И подключи в CSS:

```css
@font-face {
    font-family: 'Roboto';
    src: url('fonts/Roboto-Regular.ttf');
}

body {
    font-family: 'Roboto', Arial, sans-serif;
}
```

Не забудь добавить файл шрифта в `meta.xml`:

```xml
<file src="cef/fonts/Roboto-Regular.ttf" />
```

---

### 7. Не используй внешние библиотеки через CDN

Не будет работать:

```html
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>
```

Если нужна библиотека — скачай файл и положи локально.

---

### 8. Используй простой JavaScript

Вместо:

```javascript
fetch('/api/data').then(res => res.json())
```

Используй:

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api/data", true);
xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
        var data = JSON.parse(xhr.responseText);
    }
};
xhr.send();
```

---

## Частые ошибки

### Ошибка 1 — забыл прозрачный фон
```
Если не указать `background: transparent`, весь экран закроется цветом.
```
---

### Ошибка 2 — забыл добавить файл в meta.xml

Каждый файл CEF должен быть указан:

```xml
<file src="cef/index.html" />
<file src="cef/style.css" />
<file src="cef/script.js" />
```

Если файл не указан — клиент его не скачает и интерфейс не появится.

---

### Ошибка 3 — использовал CDN

Внешние ссылки не работают. Всё должно быть локально.

---

### Ошибка 4 — использовал современный JS

Некоторые конструкции не поддерживаются в Chromium 57:
```
- `async/await`
- `?.` (optional chaining)
- `??` (nullish coalescing)
- `import/export`
- `fetch()`
```
Используй простой синтаксис ES6:
```
- `var` или `let`
- обычные функции
- `JSON.parse()` / `JSON.stringify()`
- `document.getElementById()`
- `setTimeout()` / `setInterval()`
```
---

### Ошибка 5 — не скрыл элемент по умолчанию

Если элемент интерфейса должен быть скрыт при загрузке:

```html
<div id="menu" style="display: none;">
```

Иначе он появится сразу при входе в игру.

---

### Ошибка 6 — забыл overflow hidden

Без этого могут появляться полосы прокрутки:

```css
body {
    overflow: hidden;
}
```

---

## Безопасные CSS-свойства

Эти свойства точно работают в Chromium 57:
```
| Свойство | Работает |
|---|---|
| `display: flex` | ✅ |
| `display: block` | ✅ |
| `display: none` | ✅ |
| `position: fixed` | ✅ |
| `position: absolute` | ✅ |
| `position: relative` | ✅ |
| `background: rgba()` | ✅ |
| `border-radius` | ✅ |
| `box-shadow` | ✅ |
| `opacity` | ✅ |
| `transform: translate()` | ✅ |
| `transition` | ✅ |
| `animation` / `@keyframes` | ✅ |
| `flex-direction` | ✅ |
| `align-items` | ✅ |
| `justify-content` | ✅ |
| `gap` | ❌ (используй margin) |
| `display: grid` | ⚠️ частично |
```
> Вместо `gap` используй `margin`:

```css
.item {
    margin-bottom: 10px;
}

.item:last-child {
    margin-bottom: 0;
}
```

---

## Безопасные JS-конструкции
```
| Конструкция | Работает |
|---|---|
| `var`, `let`, `const` | ✅ |
| `function()` | ✅ |
| `() => {}` (arrow functions) | ✅ |
| `template literals` | ✅ |
| `for`, `while`, `for...of` | ✅ |
| `JSON.parse()` | ✅ |
| `JSON.stringify()` | ✅ |
| `document.getElementById()` | ✅ |
| `document.querySelector()` | ✅ |
| `addEventListener()` | ✅ |
| `setTimeout()` | ✅ |
| `setInterval()` | ✅ |
| `XMLHttpRequest` | ✅ |
| `Promise` | ✅ |
| `fetch()` | ❌ |
| `async/await` | ❌ |
| `?.` (optional chaining) | ❌ |
| `??` (nullish coalescing) | ❌ |
| `import/export` | ❌ |
```
---

## Шаблон пустого CEF-ресурса

### Структура файлов

```text
resources\
└── myui\
    ├── meta.xml
    ├── myui.cs
    └── cef\
        ├── index.html
        ├── style.css
        └── script.js
```

---

### meta.xml

```xml
<meta>
    <info name="MyUI" version="1.0" author="Dev" />
    <script src="myui.cs" type="server" lang="csharp" />
    <file src="cef/index.html" />
    <file src="cef/style.css" />
    <file src="cef/script.js" />
</meta>
```

---

### myui.cs

```csharp
using GTANetworkServer;
using GTANetworkShared;

public class MyUI : Script
{
    public MyUI()
    {
        API.onResourceStart += OnResourceStart;
    }

    private void OnResourceStart()
    {
        API.consoleOutput("[MyUI] CEF ресурс запущен.");
    }
}
```

---

### cef/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="container">
        <p>Hello from CEF!</p>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### cef/style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: transparent;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#container {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: rgba(0, 0, 0, 0.7);
    color: #ffffff;
    padding: 10px 20px;
    border-radius: 6px;
    font-size: 14px;
}
```

---

### cef/script.js

```javascript
// Твой JavaScript код здесь
```

---

## См. также

- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
