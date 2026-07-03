# ❓ FAQ

Ответы на частые вопросы по **Glubinka MP**.

---

## Сервер

---

### Как запустить сервер?

1. Скачай **GlubinkaMP-ServerKit** из раздела Releases
2. Распакуй архив в любую папку
3. Запусти файл `start.bat`

Сервер запустится и покажет консоль.

---

### Как изменить порт сервера?

Открой файл `settings.xml`, найди строку:

```xml
<port>4499</port>
```

Замени `4499` на нужный порт. Сохрани файл и перезапусти сервер.

---

### Как изменить название сервера?

Открой файл `settings.xml`, найди строку:

```xml
<servername>Glubinka MP Server</servername>
```

Замени текст между тегами. Сохрани и перезапусти сервер.

---

### Как установить пароль на сервер?

Открой файл `settings.xml`, найди строку:

```xml
<password></password>
```

Впиши пароль:

```xml
<password>mypassword123</password>
```

Сохрани и перезапусти сервер.

---

### Как изменить максимальное количество игроков?

Открой файл `settings.xml`, найди строку:

```xml
<maxplayers>32</maxplayers>
```

Замени число. Сохрани и перезапусти сервер.

---

### Где лежат логи сервера?

Логи выводятся прямо в консоль сервера.

Из своего ресурса ты можешь писать свои логи через `Logger`:

```csharp
var log = CreateLogger("MyResource");
log.Info("Мой лог.");
```

Или через быстрые методы `GlubinkaContext`:

```csharp
Log("Мой лог.");
LogError("Ошибка!");
```

---

## Ресурсы

---

### Как добавить ресурс на сервер?

1. Создай папку в:

```
resources\мой_ресурс\
```

2. Внутри создай два файла:

```
meta.xml
мой_скрипт.cs
```

3. В `meta.xml` напиши:

```xml
<meta>
    <info name="Мой ресурс" version="1.0" author="Я" />
    <script src="мой_скрипт.cs" type="server" lang="csharp" />
</meta>
```

4. В `мой_скрипт.cs` напиши свой код.

5. Открой файл `settings.xml` и добавь ресурс в список:

```xml
<resource src="мой_ресурс" />
```

6. Перезапусти сервер.

---

### Как перезагрузить ресурс без перезапуска сервера?

Никак — консоль не поддерживает ввод текста. Нужно перезапустить сервер целиком.

---

### Ресурс не загружается. Что делать?

Проверь следующее:

1. Есть ли файл `meta.xml` в папке ресурса
2. Правильно ли указан путь к скрипту в `meta.xml`
3. Нет ли ошибок компиляции в консоли сервера
4. Добавлен ли ресурс в `settings.xml`
5. Правильно ли написано имя папки ресурса

---

### Ошибка компиляции ресурса. Что делать?

Посмотри в консоль сервера. Там будет строка с ошибкой, например:

```
Error compiling resource "мой_ресурс": CS1002 ...
```

Исправь ошибку в `.cs` файле и перезапусти сервер.

Частые причины:

- забыта точка с запятой `;`
- не закрыта скобка `{` или `}`
- неправильное имя метода или переменной
- отсутствует `using GTANetworkServer;`

---

### Можно ли использовать несколько файлов в одном ресурсе?

Да. Добавь каждый файл в `meta.xml`:

```xml
<meta>
    <info name="Мой ресурс" version="1.0" author="Я" />
    <script src="main.cs" type="server" lang="csharp" />
    <script src="commands.cs" type="server" lang="csharp" />
    <script src="events.cs" type="server" lang="csharp" />
</meta>
```

---

## Клиент

---

### Как подключиться к серверу?

1. Установи **GlubinkaMP-ClientRuntime**
2. Запусти GTA V
3. Дождись появления меню Glubinka MP
4. Введи IP-адрес и порт сервера
5. Нажми подключиться

---

### Игра крашится при подключении. Что делать?

Проверь:

1. Версия GTA V должна быть `1.0.2189.0`
2. Script Hook V должен быть совместим с этой версией
3. Файлы клиента должны лежать в правильных папках
4. В папке `scripts\` должны быть:
   - `GlubinkaMP.dll`
   - `GlubinkaMP.Shared.dll`
   - `NativeUI.dll`
5. В корне GTA V должны быть:
   - `ScriptHookVDotNet.asi`
   - `ScriptHookVDotNet.dll`
6. Папка `cef\` должна существовать и содержать `libcef.dll`

---

### Меню не появляется после запуска игры. Что делать?

Проверь:

1. Файл `ScriptHookVDotNet.asi` должен быть в корне GTA V
2. Файл `ScriptHookVDotNet.dll` должен быть в корне GTA V
3. Файл `dinput8.dll` должен быть в корне GTA V
4. Файл `ScriptHookV.dll` должен быть в корне GTA V
5. В папке `scripts\` должен быть файл `GlubinkaMP.dll`

Если всё на месте, но меню не появляется:

- проверь файл `ScriptHookVDotNet.ini`
- убедись что нет конфликтов с другими модами

---

## Код и SDK

---

### Какие namespace использовать в ресурсе?

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

### Какой .NET Framework использовать?

`.NET Framework 4.7.2`

---

### Как правильно создать ресурс?

Наследуйся от `GlubinkaContext` и переопредели `OnStart`:

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

Подробнее — в разделе [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html).

---

### Где подписываться на события?

Только в `OnStart` или в конструкторе класса:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        API.onPlayerConnected += OnPlayerConnected;
    }

    private void OnPlayerConnected(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Добро пожаловать!");
    }
}
```

Не подписывайся на события внутри команд или других методов.

---

### Как отправить сообщение в чат?

Одному игроку — через `Player`:

```csharp
player.SendMessage("~g~Привет!");
player.SendSuccess("Операция выполнена.");
player.SendError("Что-то пошло не так.");
```

Всем — через `Chat`:

```csharp
Chat.SendToAll("~y~Объявление!");
Chat.SendSuccessToAll("Сервер готов.");
```

Подробнее — в разделе [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html).

---

### Как создать команду?

```csharp
[Command("heal")]
public void HealCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
    player.SendSuccess("Ты вылечен.");
}
```

Игрок вводит в чат `/heal`.

Подробнее — в разделе [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html).

---

### Как телепортировать игрока?

```csharp
player.Teleport(new Vector3(100f, 200f, 30f));
// или
player.Teleport(100f, 200f, 30f);
```

Подробнее — в разделе [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html).

---

### Как создать транспорт?

```csharp
var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
player.PutIntoVehicle(car);
```

Подробнее — в разделе [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html).

---

### Как выдать оружие?

```csharp
player.GiveWeapon(WeaponHash.Pistol, 100);
```

Подробнее — в разделе [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html).

---

### Как сменить погоду?

```csharp
World.SetWeather(WeatherType.Rain);
```

Подробнее — в разделе [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html).

---

### Как сменить время?

```csharp
World.SetTime(12, 0);   // 12:00
World.SetNoon();        // быстрый пресет
```

Подробнее — в разделе [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html).

---

### Как сменить скин игрока?

```csharp
player.SetSkin(PedHash.FreemodeMale01);
```

Подробнее — в разделе [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html).

---

## Погода

---

### Какие значения погоды доступны?

| ID | Enum | Погода |
|---|---|---|
| 0 | `WeatherType.ExtraSunny` | Очень солнечно |
| 1 | `WeatherType.Clear` | Ясно |
| 2 | `WeatherType.Clouds` | Облачно |
| 3 | `WeatherType.Smog` | Смог |
| 4 | `WeatherType.Foggy` | Туман |
| 5 | `WeatherType.Overcast` | Пасмурно |
| 6 | `WeatherType.Rain` | Дождь |
| 7 | `WeatherType.Thunder` | Гроза |
| 8 | `WeatherType.Clearing` | Прояснение |
| 9 | `WeatherType.Neutral` | Нейтральная |
| 10 | `WeatherType.Snow` | Снег |
| 11 | `WeatherType.Blizzard` | Метель |
| 12 | `WeatherType.Snowlight` | Лёгкий снег |
| 13 | `WeatherType.Xmas` | Рождественская |

> Не используй значения больше `13`. Это может вызвать проблемы.

---

## CEF и интерфейсы

---

### Какая версия CEF используется?

```
CEF 3.2987 / Chromium 57 (2017)
```

---

### Какие технологии поддерживаются в CEF?

- HTML5
- CSS3 (включая flexbox)
- JavaScript (ES6 базовый)
- Локальные файлы

---

### Чего нет в этой версии CEF?

- CSS Grid (частично)
- ES2017+ (async/await могут не работать)
- Современные Web API
- Fetch API — используй `XMLHttpRequest`

---

### Какой стек рекомендуется для UI?

- HTML + CSS + vanilla JavaScript
- Flexbox для вёрстки
- Локальные файлы (без внешних CDN)
- Минимум зависимостей

---

## Разное

---

### Можно ли использовать базу данных?

Да. Ты можешь подключить любую .NET-совместимую библиотеку:

- SQLite
- MySQL (через `MySqlConnector` или `MySql.Data`)
- LiteDB
- JSON-файлы как простое хранилище

Добавь нужную DLL в папку ресурса и укажи в `meta.xml`:

```xml
<reference src="MySql.Data.dll" />
```

---

### Можно ли использовать внешние DLL?

Да. Положи DLL в папку ресурса и добавь в `meta.xml`:

```xml
<reference src="MyLibrary.dll" />
```

---

### Где найти примеры ресурсов?

Примеры включены в ServerKit в папке:

```
resources\helloworld\
```

Дополнительные примеры есть в документации:

- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)

---

### Как узнать хэш модели транспорта?

Используй готовый enum `VehicleHash`:

```csharp
VehicleHash model = VehicleHash.Adder;
```

---

### Как узнать хэш модели пешехода?

Используй готовый enum `PedHash`:

```csharp
PedHash model = PedHash.Cop01SFY;
```

---

### Как узнать хэш оружия?

Используй готовый enum `WeaponHash`:

```csharp
WeaponHash weapon = WeaponHash.Pistol;
```

---

## Цветовые коды для чата

| Код | Цвет |
|---|---|
| `~r~` | Красный |
| `~g~` | Зелёный |
| `~b~` | Синий |
| `~y~` | Жёлтый |
| `~w~` | Белый |
| `~p~` | Фиолетовый |
| `~o~` | Оранжевый |

Удобнее использовать константы `ChatColor`:

```csharp
player.SendMessage(ChatColor.Green + "Успех!");
Chat.SendToAll(ChatColor.Yellow + "Объявление!");
```

Подробнее — в разделе [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html).

---

## См. также

- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
