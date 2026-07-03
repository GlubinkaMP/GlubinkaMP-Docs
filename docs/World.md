# World

Документация по управлению миром в **Glubinka MP SDK**.

`World` — это статический класс для управления погодой, временем, IPL, данными мира и настройками сервера.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Важно

`World` — статический класс. Его не нужно создавать через `new`.

Он доступен сразу внутри любого ресурса, унаследованного от `GlubinkaContext`.

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        World.SetWeather(WeatherType.Clear);
        World.SetTime(12, 0);
    }
}
```

> Использовать `World` вне `GlubinkaContext` нельзя — он не будет инициализирован.

---

## Погода

### Установить погоду

```csharp
World.SetWeather(WeatherType.Rain);     // через enum
World.SetWeather(6);                    // через числовой ID
World.Weather = WeatherType.Snow;       // через свойство
World.WeatherId = 10;                   // через числовой ID свойства
```

### Прочитать текущую погоду

```csharp
WeatherType weather = World.Weather;
int weatherId       = World.WeatherId;
string weatherName  = World.WeatherName;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Weather` | `WeatherType` | Текущая погода (чтение / запись) |
| `WeatherId` | `int` | Текущая погода числом (чтение / запись) |
| `WeatherName` | `string` | Название текущей погоды |

### Получить название погоды

```csharp
string name = World.GetWeatherName(WeatherType.Rain);
string name = World.GetWeatherName(6);
```

### Все типы погоды (`WeatherType`)

| Значение enum | Число | Описание |
|---|---|---|
| `WeatherType.ExtraSunny` | 0 | Очень солнечно |
| `WeatherType.Clear` | 1 | Ясно |
| `WeatherType.Clouds` | 2 | Облачно |
| `WeatherType.Smog` | 3 | Смог |
| `WeatherType.Foggy` | 4 | Туман |
| `WeatherType.Overcast` | 5 | Пасмурно |
| `WeatherType.Rain` | 6 | Дождь |
| `WeatherType.Thunder` | 7 | Гроза |
| `WeatherType.Clearing` | 8 | Проясняется |
| `WeatherType.Neutral` | 9 | Нейтральная |
| `WeatherType.Snow` | 10 | Снег |
| `WeatherType.Blizzard` | 11 | Метель |
| `WeatherType.Snowlight` | 12 | Лёгкий снег |
| `WeatherType.Xmas` | 13 | Рождественская |

### Пример

```csharp
[Command("weather")]
public void WeatherCommand(Client client, string type)
{
    var player = WrapPlayer(client);

    switch (type.ToLower())
    {
        case "rain":
            World.SetWeather(WeatherType.Rain);
            player.SendMessage("~b~Погода: дождь.");
            break;
        case "sun":
            World.SetWeather(WeatherType.ExtraSunny);
            player.SendMessage("~y~Погода: солнечно.");
            break;
        case "snow":
            World.SetWeather(WeatherType.Snow);
            player.SendMessage("~w~Погода: снег.");
            break;
        default:
            player.SendError("Неизвестная погода.");
            break;
    }
}
```

---

## Время

### Установить время

```csharp
World.SetTime(18, 30);                        // час и минуты
World.SetTime(new TimeSpan(18, 30, 0));       // через TimeSpan
World.CurrentTime = new TimeSpan(12, 0, 0);  // через свойство
```

### Прочитать текущее время

```csharp
TimeSpan time = World.CurrentTime;
TimeSpan time = World.GetTime();
int hour      = World.Hour;
int minute    = World.Minute;
```

| Свойство | Тип | Описание |
|---|---|---|
| `CurrentTime` | `TimeSpan` | Текущее время (чтение / запись) |
| `Hour` | `int` | Текущий час (0–23) |
| `Minute` | `int` | Текущая минута (0–59) |

### Быстрые пресеты времени

```csharp
World.SetMorning();   // 09:00
World.SetNoon();      // 12:00
World.SetEvening();   // 18:00
World.SetNight();     // 00:00
```

| Метод | Время | Описание |
|---|---|---|
| `SetMorning()` | 09:00 | Утро |
| `SetNoon()` | 12:00 | Полдень |
| `SetEvening()` | 18:00 | Вечер |
| `SetNight()` | 00:00 | Ночь |

### Пример

```csharp
[Command("time")]
public void TimeCommand(Client client, int hour, int minute)
{
    var player = WrapPlayer(client);
    World.SetTime(hour, minute);
    player.SendMessage("~y~Время установлено: " + hour + ":" + minute.ToString("D2"));
}
```

---

## Пресеты погоды и времени

Можно установить погоду и время одновременно одной командой.

### Готовые пресеты

```csharp
World.SetSunnyMorning();   // ExtraSunny, 09:00
World.SetClearDay();       // Clear, 12:00
World.SetFoggyMorning();   // Foggy, 07:00
World.SetRainyNight();     // Rain, 02:00
World.SetSnowyNight();     // Snow, 23:00
```

| Метод | Погода | Время | Описание |
|---|---|---|---|
| `SetSunnyMorning()` | ExtraSunny | 09:00 | Солнечное утро |
| `SetClearDay()` | Clear | 12:00 | Ясный день |
| `SetFoggyMorning()` | Foggy | 07:00 | Туманное утро |
| `SetRainyNight()` | Rain | 02:00 | Дождливая ночь |
| `SetSnowyNight()` | Snow | 23:00 | Снежная ночь |

### Своя комбинация

```csharp
World.SetWeatherAndTime(WeatherType.Thunder, 3, 0);  // гроза в 03:00
World.SetWeatherAndTime(7, 3, 0);                    // то же самое через число
```

### Пример

```csharp
[Command("preset")]
public void Preset(Client client, string name)
{
    var player = WrapPlayer(client);

    switch (name.ToLower())
    {
        case "morning":
            World.SetSunnyMorning();
            player.SendSuccess("Пресет: солнечное утро.");
            break;
        case "night":
            World.SetRainyNight();
            player.SendSuccess("Пресет: дождливая ночь.");
            break;
        case "snow":
            World.SetSnowyNight();
            player.SendSuccess("Пресет: снежная ночь.");
            break;
        default:
            player.SendError("Неизвестный пресет.");
            break;
    }
}
```

---

## IPL

IPL — это загружаемые локации и интерьеры в GTA V (например, казино, бункер и т.п.).

### Загрузить IPL

```csharp
World.RequestIpl("hei_bi_hw1_13");
```

### Выгрузить IPL

```csharp
World.RemoveIpl("hei_bi_hw1_13");
```

### Сбросить все IPL

```csharp
World.ResetIplList();
```

| Метод | Описание |
|---|---|
| `RequestIpl(string)` | Загрузить IPL |
| `RemoveIpl(string)` | Выгрузить IPL |
| `ResetIplList()` | Сбросить все загруженные IPL |

### Пример

```csharp
protected override void OnStart()
{
    World.RequestIpl("hei_bi_hw1_13");
    Log("IPL загружен.");
}
```

> Название IPL не может быть пустым — SDK выбросит ошибку.

---

## World Data

World Data — это серверные данные, привязанные к миру. Они существуют на сервере и **не синхронизируются** клиентам автоматически.

Используй для хранения глобального состояния ресурса: режима сервера, флагов, счётчиков и т.п.

### Записать данные

```csharp
World.SetData("server_mode", "roleplay");
World.SetData("event_active", true);
World.SetData("max_wanted", 3);
```

### Прочитать данные

```csharp
object raw    = World.GetData("server_mode");
string mode   = World.GetData<string>("server_mode");
bool active   = World.GetData<bool>("event_active");
int maxWanted = World.GetData<int>("max_wanted", 5);  // с дефолтным значением
```

### Проверить существование ключа

```csharp
bool exists = World.HasData("server_mode");
```

### Удалить ключ

```csharp
World.ResetData("server_mode");
```

| Метод | Описание |
|---|---|
| `SetData(string, object)` | Записать данные |
| `GetData(string)` | Прочитать как object |
| `GetData<T>(string)` | Прочитать с приведением типа |
| `GetData<T>(string, T)` | Прочитать с дефолтным значением |
| `HasData(string)` | Проверить существование ключа |
| `ResetData(string)` | Удалить ключ |
| `GetAllData()` | Получить все данные |

### Try API

```csharp
if (World.TryGetData<string>("server_mode", out string mode))
{
    Log("Режим сервера: " + mode);
}
```

### Пример

```csharp
protected override void OnStart()
{
    World.SetData("server_mode", "roleplay");
    World.SetData("event_active", false);
    Log("Данные мира инициализированы.");
}

[Command("servermode")]
public void ServerMode(Client client)
{
    var player = WrapPlayer(client);
    string mode = World.GetData<string>("server_mode", "unknown");
    player.SendMessage("~y~Режим сервера: " + mode);
}
```

---

## World Synced Data

Synced Data — это данные мира, которые **синхронизируются клиентам**. Используй их, когда клиентским скриптам нужно знать о каком-то глобальном состоянии сервера.

### Записать synced данные

```csharp
World.SetSyncedData("global_event", "race");
World.SetSyncedData("pvp_enabled", true);
```

### Прочитать synced данные

```csharp
object raw     = World.GetSyncedData("global_event");
string ev      = World.GetSyncedData<string>("global_event");
bool pvp       = World.GetSyncedData<bool>("pvp_enabled");
int val        = World.GetSyncedData<int>("some_number", 0); // с дефолтным значением
```

### Проверить существование ключа

```csharp
bool exists = World.HasSyncedData("global_event");
```

### Удалить ключ

```csharp
World.ResetSyncedData("global_event");
```

| Метод | Описание |
|---|---|
| `SetSyncedData(string, object)` | Записать synced данные |
| `GetSyncedData(string)` | Прочитать как object |
| `GetSyncedData<T>(string)` | Прочитать с приведением типа |
| `GetSyncedData<T>(string, T)` | Прочитать с дефолтным значением |
| `HasSyncedData(string)` | Проверить существование ключа |
| `ResetSyncedData(string)` | Удалить ключ |
| `GetAllSyncedData()` | Получить все synced данные |

### Try API

```csharp
if (World.TryGetSyncedData<string>("global_event", out string ev))
{
    Log("Текущее событие: " + ev);
}
```

### Разница между Data и SyncedData

| | World Data | World Synced Data |
|---|---|---|
| Видна клиентам | ❌ Нет | ✅ Да |
| Используется для | Серверного состояния | Глобальных данных для клиентов |

### Пример

```csharp
[Command("startevent")]
public void StartEvent(Client client, string eventName)
{
    var player = WrapPlayer(client);
    World.SetSyncedData("global_event", eventName);
    World.SetData("event_active", true);
    player.SendSuccess("Событие запущено: " + eventName);
}

[Command("stopevent")]
public void StopEvent(Client client)
{
    var player = WrapPlayer(client);
    World.ResetSyncedData("global_event");
    World.SetData("event_active", false);
    player.SendSuccess("Событие остановлено.");
}
```

---

## Данные сервера

### Прочитать данные сервера

```csharp
string name      = World.ServerName;
int maxPlayers   = World.MaxPlayers;
int port         = World.ServerPort;
bool hasPassword = World.PasswordProtected;
```

| Свойство | Тип | Описание |
|---|---|---|
| `ServerName` | `string` | Имя сервера (чтение / запись) |
| `MaxPlayers` | `int` | Максимум игроков |
| `ServerPort` | `int` | Порт сервера |
| `PasswordProtected` | `bool` | Защищён ли сервер паролем |

### Изменить имя сервера

```csharp
World.ServerName = "Glubinka RP";
World.SetServerName("Glubinka RP");  // alias
```

### Управление паролем

```csharp
World.SetServerPassword("mypassword");  // установить пароль
World.ClearServerPassword();            // снять пароль
World.EnablePassword("mypassword");     // alias для SetServerPassword
World.DisablePassword();                // alias для ClearServerPassword
```

| Метод | Описание |
|---|---|
| `SetServerName(string)` | Установить имя сервера |
| `SetServerPassword(string)` | Установить пароль |
| `ClearServerPassword()` | Снять пароль |
| `EnablePassword(string)` | Alias для SetServerPassword |
| `DisablePassword()` | Alias для ClearServerPassword |

### Пример

```csharp
protected override void OnStart()
{
    World.SetServerName("Glubinka RP | Beta");
    Log("Сервер: " + World.ServerName);
    Log("Макс. игроков: " + World.MaxPlayers);
    Log("Порт: " + World.ServerPort);
}
```

---

## Утилиты

### Получить GTA hash key строки

```csharp
int hash = World.GetHashKey("WEAPON_PISTOL");
```

### Получить SHA256-хэш строки

```csharp
string hash = World.GetHashSHA256("mypassword");
```

| Метод | Описание |
|---|---|
| `GetHashKey(string)` | GTA hash key по строке |
| `GetHashSHA256(string)` | SHA256-хэш строки |

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class WorldDemo : GlubinkaContext
{
    protected override void OnStart()
    {
        World.SetServerName("Glubinka RP");
        World.SetSunnyMorning();
        World.SetData("server_mode", "roleplay");
        World.SetSyncedData("pvp_enabled", false);
        World.RequestIpl("hei_bi_hw1_13");

        Log("Мир инициализирован.");
        Log("Сервер: " + World.ServerName);
        Log("Погода: " + World.WeatherName);
        Log("Время: " + World.Hour + ":" + World.Minute.ToString("D2"));
    }

    [Command("weather")]
    public void WeatherCmd(Client client, int id)
    {
        var player = WrapPlayer(client);
        World.SetWeather(id);
        player.SendMessage("~y~Погода: " + World.GetWeatherName(id));
    }

    [Command("time")]
    public void TimeCmd(Client client, int hour, int minute)
    {
        var player = WrapPlayer(client);
        World.SetTime(hour, minute);
        player.SendMessage("~y~Время: " + hour + ":" + minute.ToString("D2"));
    }

    [Command("preset")]
    public void PresetCmd(Client client, string name)
    {
        var player = WrapPlayer(client);

        switch (name.ToLower())
        {
            case "morning":
                World.SetSunnyMorning();
                player.SendSuccess("Пресет: солнечное утро.");
                break;
            case "day":
                World.SetClearDay();
                player.SendSuccess("Пресет: ясный день.");
                break;
            case "rain":
                World.SetRainyNight();
                player.SendSuccess("Пресет: дождливая ночь.");
                break;
            case "snow":
                World.SetSnowyNight();
                player.SendSuccess("Пресет: снежная ночь.");
                break;
            default:
                player.SendError("Доступные пресеты: morning, day, rain, snow.");
                break;
        }
    }

    [Command("serverinfo")]
    public void ServerInfo(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~y~Сервер: " + World.ServerName);
        player.SendMessage("~w~Макс. игроков: " + World.MaxPlayers);
        player.SendMessage("~w~Порт: " + World.ServerPort);
        player.SendMessage("~w~Пароль: " + (World.PasswordProtected ? "Да" : "Нет"));
    }
}
```

---

## Частые ошибки

### Ошибка 1 — использовал World вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void DoSomething()
    {
        World.SetWeather(WeatherType.Rain);
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        World.SetWeather(WeatherType.Rain);
    }
}
```

---

### Ошибка 2 — передал неверный ID погоды

Неправильно:

```csharp
World.SetWeather(99);
```

Правильно:

```csharp
World.SetWeather(WeatherType.Rain);
// или
World.SetWeather(6);  // допустимые значения: 0-13
```

---

### Ошибка 3 — передал неверное время

Неправильно:

```csharp
World.SetTime(25, 70);
```

Правильно:

```csharp
World.SetTime(23, 59);  // час 0-23, минуты 0-59
```

---

### Ошибка 4 — установил пустой пароль через EnablePassword

Неправильно:

```csharp
World.EnablePassword("");
```

Правильно:

```csharp
World.EnablePassword("mypassword");
// или для снятия пароля:
World.DisablePassword();
```

---

### Ошибка 5 — не проверил ключ перед получением данных

Неправильно:

```csharp
string mode = World.GetData<string>("server_mode");
```

Правильно — с дефолтным значением:

```csharp
string mode = World.GetData<string>("server_mode", "unknown");
```

Или через Try API:

```csharp
if (World.TryGetData<string>("server_mode", out string mode))
{
    Log("Режим: " + mode);
}
```

---

## Все методы World — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Weather` | Текущая погода (чтение / запись) |
| `WeatherId` | Текущая погода числом (чтение / запись) |
| `WeatherName` | Название текущей погоды |
| `SetWeather(WeatherType)` | Установить погоду через enum |
| `SetWeather(int)` | Установить погоду через число |
| `GetWeatherName(WeatherType)` | Название погоды по enum |
| `GetWeatherName(int)` | Название погоды по числу |
| `CurrentTime` | Текущее время (чтение / запись) |
| `Hour` | Текущий час |
| `Minute` | Текущая минута |
| `GetTime()` | Получить текущее время |
| `SetTime(int, int)` | Установить время |
| `SetTime(TimeSpan)` | Установить время через TimeSpan |
| `SetMorning()` | Установить утро (09:00) |
| `SetNoon()` | Установить полдень (12:00) |
| `SetEvening()` | Установить вечер (18:00) |
| `SetNight()` | Установить ночь (00:00) |
| `SetWeatherAndTime(WeatherType, int, int)` | Погода и время сразу |
| `SetWeatherAndTime(int, int, int)` | Погода числом и время сразу |
| `SetSunnyMorning()` | Пресет: солнечное утро |
| `SetClearDay()` | Пресет: ясный день |
| `SetFoggyMorning()` | Пресет: туманное утро |
| `SetRainyNight()` | Пресет: дождливая ночь |
| `SetSnowyNight()` | Пресет: снежная ночь |
| `RequestIpl(string)` | Загрузить IPL |
| `RemoveIpl(string)` | Выгрузить IPL |
| `ResetIplList()` | Сбросить все IPL |
| `SetData(string, object)` | Записать world data |
| `GetData(string)` | Прочитать world data |
| `GetData<T>(string)` | Прочитать с типом |
| `GetData<T>(string, T)` | Прочитать с дефолтом |
| `HasData(string)` | Проверить ключ |
| `ResetData(string)` | Удалить ключ |
| `GetAllData()` | Все world data |
| `TryGetData<T>(string, out T)` | Безопасное чтение |
| `SetSyncedData(string, object)` | Записать synced data |
| `GetSyncedData(string)` | Прочитать synced data |
| `GetSyncedData<T>(string)` | Прочитать synced с типом |
| `GetSyncedData<T>(string, T)` | Прочитать synced с дефолтом |
| `HasSyncedData(string)` | Проверить synced ключ |
| `ResetSyncedData(string)` | Удалить synced ключ |
| `GetAllSyncedData()` | Все synced data |
| `TryGetSyncedData<T>(string, out T)` | Безопасное чтение synced |
| `ServerName` | Имя сервера (чтение / запись) |
| `MaxPlayers` | Максимум игроков |
| `ServerPort` | Порт сервера |
| `PasswordProtected` | Защищён ли паролем |
| `SetServerName(string)` | Установить имя сервера |
| `SetServerPassword(string)` | Установить пароль |
| `ClearServerPassword()` | Снять пароль |
| `EnablePassword(string)` | Alias для SetServerPassword |
| `DisablePassword()` | Alias для ClearServerPassword |
| `GetHashKey(string)` | GTA hash key строки |
| `GetHashSHA256(string)` | SHA256-хэш строки |

---

## См. также
