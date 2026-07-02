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

Открой файл:

```
settings.xml
```

Найди строку:

```
<port>4499</port>
```

Замени `4499` на нужный порт. Сохрани файл и перезапусти сервер.

---

### Как изменить название сервера?

Открой файл:

```
settings.xml
```

Найди строку:

```
<servername>Glubinka MP Server</servername>
```

Замени текст между тегами. Сохрани и перезапусти сервер.

---

### Как установить пароль на сервер?

Открой файл:

```
settings.xml
```

Найди строку:

```
<password></password>
```

Впиши пароль между тегами:

```
<password>mypassword123</password>
```

Сохрани и перезапусти сервер.

---

### Как изменить максимальное количество игроков?

Открой файл:

```
settings.xml
```

Найди строку:

```
<maxplayers>32</maxplayers>
```

Замени число. Сохрани и перезапусти сервер.

---

### Где лежат логи сервера?

Логи сервера обычно выводятся прямо в консоль.

Также ты можешь писать свои логи через:

```
API.consoleOutput("Мой лог.");
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

```text
meta.xml
мой_скрипт.cs
```

3. В `meta.xml` напиши:

```
<meta>
    <info name="Мой ресурс" version="1.0" author="Я" />
    <script src="мой_скрипт.cs" type="server" lang="csharp" />
</meta>
```

4. В `мой_скрипт.cs` напиши свой код.

5. Открой файл `settings.xml` и добавь ресурс в список:

```
<resource src="мой_ресурс" />
```

6. Перезапусти сервер.

---

### Как перезагрузить ресурс без перезапуска сервера?

Никак, консоль не поддерживает ввод текста

---

### Ресурс не загружается. Что делать?

Проверь:
```
1. Есть ли файл `meta.xml` в папке ресурса
2. Правильно ли указан путь к скрипту в `meta.xml`
3. Нет ли ошибок компиляции в консоли сервера
4. Добавлен ли ресурс в `settings.xml`
5. Правильно ли написано имя папки ресурса
```
---

### Ошибка компиляции ресурса. Что делать?

Посмотри в консоль сервера. Там будет строка с ошибкой, например:

```
Error compiling resource "мой_ресурс": CS1002 ...
```

Исправь ошибку в `.cs` файле и перезапусти ресурс.

Частые причины:
```
- забыта точка с запятой `;`
- не закрыта скобка `{` или `}`
- неправильное имя метода или переменной
- отсутствует `using GTANetworkServer;`
```
---

### Можно ли использовать несколько файлов в одном ресурсе?

Да. Добавь каждый файл в `meta.xml`:

```
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
```
1. Установи **GlubinkaMP-ClientRuntime**
2. Запусти GTA V
3. Дождись появления меню Glubinka MP
4. Введи IP-адрес и порт сервера
5. Нажми подключиться
```
---

### Игра крашится при подключении. Что делать?

Проверь:
```
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
```
---

### Меню не появляется после запуска игры. Что делать?

Проверь:
```
1. Файл `ScriptHookVDotNet.asi` должен быть в корне GTA V
2. Файл `ScriptHookVDotNet.dll` должен быть в корне GTA V
3. Файл `dinput8.dll` должен быть в корне GTA V
4. Файл `ScriptHookV.dll` должен быть в корне GTA V
5. В папке `scripts\` должен быть файл `GlubinkaMP.dll`
```
Если всё на месте но меню не появляется:
```
- проверь файл `ScriptHookVDotNet.ini`
- убедись что нет конфликтов с другими модами
```
---

## Код и API

---

### Какие namespace использовать в ресурсе?

```
using GTANetworkServer;
using GTANetworkShared;
```

Это внутренние имена, оставленные для совместимости. Так и должно быть.

---

### Какой .NET Framework использовать?

```
.NET Framework 4.7.2
```

---

### Где подписываться на события?

Только в конструкторе класса:

```
public class MyResource : Script
{
    public MyResource()
    {
        API.onResourceStart += OnResourceStart;
    }

    private void OnResourceStart()
    {
    }
}
```

Не подписывайся на события внутри других методов.

---

### Как отправить сообщение в чат?

Одному игроку:

```
API.sendChatMessageToPlayer(player, "~g~Привет!");
```

Всем:

```
API.sendChatMessageToAll("~y~Объявление!");
```

---

### Как создать команду?

```
[Command("mycommand")]
public void MyCommand(Client player)
{
    API.sendChatMessageToPlayer(player, "~g~Команда работает!");
}
```

Игрок вводит в чат:

```
/mycommand
```

---

### Как телепортировать игрока?

```
API.setEntityPosition(player.handle, new Vector3(100f, 200f, 30f));
```

---

### Как создать транспорт?

```
API.createVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0), 0, 0);
```

---

### Как выдать оружие?

```
API.givePlayerWeapon(player, WeaponHash.Pistol, 100, true, true);
```

---

### Как сменить погоду?

```
API.setWeather(1);
```

Допустимые значения: от 0 до 13.

---

### Как сменить время?

```
API.setTime(12, 0);
```

Первый параметр — часы (0-23), второй — минуты (0-59).

---

### Как сменить скин игрока?

```
API.setPlayerSkin(player, PedHash.FreemodeMale01);
```

---

## Погода

---

### Какие ID погоды доступны?
```
| ID | Погода |
|---|---|
| 0 | EXTRASUNNY |
| 1 | CLEAR |
| 2 | CLOUDS |
| 3 | SMOG |
| 4 | FOGGY |
| 5 | OVERCAST |
| 6 | RAIN |
| 7 | THUNDER |
| 8 | CLEARING |
| 9 | NEUTRAL |
| 10 | SNOW |
| 11 | BLIZZARD |
| 12 | SNOWLIGHT |
| 13 | XMAS |
```
> ⚠️ Не используй значения больше 13. Это может вызвать проблемы.

---

## CEF и интерфейсы

---

### Какая версия CEF используется?

```
CEF 3.2987 / Chromium 57 (2017)
```

---

### Какие технологии поддерживаются в CEF?
```
- HTML5
- CSS3 (включая flexbox)
- JavaScript (ES6 базовый)
- Локальные файлы
```
---

### Чего нет в этой версии CEF?
```
- CSS Grid (частично)
- ES2017+ (async/await могут не работать)
- Современные Web API
- Fetch API (используй XMLHttpRequest)
```
---

### Какой стек рекомендуется для UI?
```
- HTML + CSS + vanilla JavaScript
- Flexbox для вёрстки
- Локальные файлы (без внешних CDN)
- Минимум зависимостей
```
---

## Разное

---

### Можно ли использовать базу данных?

Да. Ты можешь подключить любую .NET-совместимую библиотеку для работы с БД:
```
- SQLite
- MySQL (через MySqlConnector или MySql.Data)
- LiteDB
- JSON-файлы как простое хранилище
```
Добавь нужную DLL в папку ресурса и укажи в `meta.xml`:

```
<reference src="MySql.Data.dll" />
```

---

### Можно ли использовать внешние DLL?

Да. Положи DLL в папку ресурса и добавь в `meta.xml`:

```
<reference src="MyLibrary.dll" />
```

---

### Где найти примеры ресурсов?

Примеры включены в ServerKit в папке:

```
resources\helloworld\
```

Дополнительные примеры можно найти в документации:
```
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
```
---

### Как узнать хэш модели транспорта?

Используй функцию:

```
VehicleHash model = API.vehicleNameToModel("adder");
```

Или используй готовый enum `VehicleHash`:

```
VehicleHash model = VehicleHash.Adder;
```

---

### Как узнать хэш модели пешехода?

```
PedHash model = API.pedNameToModel("cop01sfy");
```

Или:

```
PedHash model = PedHash.Cop01SFY;
```

---

### Как узнать хэш оружия?

```
WeaponHash weapon = API.weaponNameToModel("pistol");
```

Или:

```
WeaponHash weapon = WeaponHash.Pistol;
```

---

## Цветовые коды для чата
```
| Код | Цвет |
|---|---|
| `~r~` | Красный |
| `~g~` | Зелёный |
| `~b~` | Синий |
| `~y~` | Жёлтый |
| `~w~` | Белый |
| `~p~` | Фиолетовый |
| `~o~` | Оранжевый |
```
---

## См. также
```
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
```
