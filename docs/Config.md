---
title: Config
nav_order: 11
---

# Config

Документация по работе с файлом настроек в **Glubinka MP SDK**.

`Config` — это класс для чтения настроек ресурса из простого текстового файла формата `ключ=значение`. Удобен для хранения параметров ресурса без необходимости менять код.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
```

---

## Формат файла настроек

Файл настроек — это обычный текстовый файл. Каждая строка — это пара `ключ=значение`.

### Пример `config.ini`

```ini
# Это комментарий — строки с # игнорируются

welcome_message=Добро пожаловать на сервер!
max_players=32
pvp_enabled=true
spawn_x=-269.0
spawn_y=-955.0
spawn_z=31.0
server_name=Glubinka RP
debug_mode=false
```

### Правила формата

| Правило | Описание |
|---|---|
| `ключ=значение` | Основной формат строки |
| `# комментарий` | Строки начинающиеся с `#` игнорируются |
| Пустые строки | Пропускаются |
| Регистр ключей | Не важен — `PvP_Enabled` и `pvp_enabled` одно и то же |
| Разделитель дробных чисел | Точка: `1.5`, не запятая |

---

## Как создать Config

```csharp
var config = new Config("config.ini");
```

Путь может быть относительным (от папки ресурса) или абсолютным.

```csharp
var config = new Config("resources/myresource/config.ini");
```

> Если файл не найден — `Config` не выбросит ошибку. Он будет работать с пустыми значениями и возвращать дефолты.

### Рекомендация

Создавай `Config` один раз в `OnStart` и храни как поле класса.

```csharp
public class MyResource : GlubinkaContext
{
    private Config _config;

    protected override void OnStart()
    {
        _config = new Config("config.ini");
        Log("Настройки загружены. Параметров: " + _config.Count);
    }
}
```

---

## Свойства

```csharp
string path  = config.FilePath;
bool exists  = config.FileExists;
int count    = config.Count;
```

| Свойство | Тип | Описание |
|---|---|---|
| `FilePath` | `string` | Путь к файлу настроек |
| `FileExists` | `bool` | Существует ли файл на диске |
| `Count` | `int` | Количество загруженных параметров |

---

## Чтение строки

```csharp
string msg = config.Get("welcome_message");
string msg = config.Get("welcome_message", "Привет!");   // с дефолтным значением
```

> Если ключ не найден — возвращает дефолтное значение. По умолчанию дефолт — пустая строка `""`.

---

## Чтение числа (int)

```csharp
int maxPlayers = config.GetInt("max_players");
int maxPlayers = config.GetInt("max_players", 32);   // с дефолтным значением
```

> Если значение не является числом — возвращает дефолт.

---

## Чтение дробного числа (float)

```csharp
float x = config.GetFloat("spawn_x");
float x = config.GetFloat("spawn_x", 0f);   // с дефолтным значением
```

> В файле используй точку как разделитель: `spawn_x=-269.0`

---

## Чтение дробного числа (double)

```csharp
double value = config.GetDouble("some_value");
double value = config.GetDouble("some_value", 0.0);
```

---

## Чтение булевого значения (bool)

```csharp
bool pvp = config.GetBool("pvp_enabled");
bool pvp = config.GetBool("pvp_enabled", false);   // с дефолтным значением
```

### Допустимые значения в файле для bool

| Значение в файле | Результат |
|---|---|
| `true` | `true` |
| `1` | `true` |
| `yes` | `true` |
| `on` | `true` |
| `false` | `false` |
| `0` | `false` |
| `no` | `false` |
| `off` | `false` |

> Регистр не важен — `True`, `TRUE`, `true` — всё одно и то же.

---

## Проверить существование ключа

```csharp
bool exists = config.HasKey("welcome_message");
```

---

## Перезагрузить файл

Если файл был изменён во время работы сервера — можно перечитать его без перезапуска ресурса.

```csharp
config.Reload();
```

---

## Try API — безопасное чтение

Try-методы возвращают `true` если ключ найден и значение успешно прочитано.

```csharp
if (config.TryGet("welcome_message", out string msg))
{
    Chat.SendToAll(msg);
}

if (config.TryGetInt("max_players", out int maxPlayers))
{
    Log("Максимум игроков: " + maxPlayers);
}

if (config.TryGetFloat("spawn_x", out float x))
{
    Log("Spawn X: " + x);
}

if (config.TryGetBool("pvp_enabled", out bool pvp))
{
    Log("PvP: " + pvp);
}
```

| Метод | Описание |
|---|---|
| `TryGet(string, out string)` | Безопасное чтение строки |
| `TryGetInt(string, out int)` | Безопасное чтение числа |
| `TryGetFloat(string, out float)` | Безопасное чтение дробного числа |
| `TryGetBool(string, out bool)` | Безопасное чтение булевого значения |

---

## Все методы чтения — сводная таблица

| Метод | Возвращает | Описание |
|---|---|---|
| `Get(string, string)` | `string` | Строковое значение |
| `GetInt(string, int)` | `int` | Целое число |
| `GetFloat(string, float)` | `float` | Дробное число |
| `GetDouble(string, double)` | `double` | Дробное число (double) |
| `GetBool(string, bool)` | `bool` | Булевое значение |
| `TryGet(string, out string)` | `bool` | Безопасное чтение строки |
| `TryGetInt(string, out int)` | `bool` | Безопасное чтение числа |
| `TryGetFloat(string, out float)` | `bool` | Безопасное чтение дробного |
| `TryGetBool(string, out bool)` | `bool` | Безопасное чтение булевого |
| `HasKey(string)` | `bool` | Существует ли ключ |
| `Reload()` | `void` | Перечитать файл заново |

---

## Полный пример

### Файл `config.ini`

```ini
# Настройки сервера
server_name=Glubinka RP
welcome_message=Добро пожаловать на Glubinka RP!
max_players=64
pvp_enabled=true
debug_mode=false

# Точка спавна
spawn_x=-269.0
spawn_y=-955.0
spawn_z=31.0

# Экономика
start_money=1000
tax_rate=0.15
```

### Ресурс

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class ConfigDemo : GlubinkaContext
{
    private Config _config;
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("ConfigDemo");
        _config = new Config("config.ini");

        if (!_config.FileExists)
        {
            _log.Warning("Файл config.ini не найден. Используются значения по умолчанию.");
        }
        else
        {
            _log.Info("Настройки загружены. Параметров: {0}", _config.Count);
        }

        // Читаем настройки
        string serverName = _config.Get("server_name", "Glubinka MP");
        string welcomeMsg = _config.Get("welcome_message", "Добро пожаловать!");
        int maxPlayers    = _config.GetInt("max_players", 32);
        bool pvp          = _config.GetBool("pvp_enabled", false);
        bool debug        = _config.GetBool("debug_mode", false);

        // Применяем
        World.SetServerName(serverName);
        World.SetSyncedData("pvp_enabled", pvp);

        _log.Info("Сервер: {0}", serverName);
        _log.Info("Макс. игроков: {0}", maxPlayers);
        _log.Info("PvP: {0}", pvp);
        _log.Info("Отладка: {0}", debug);

        // Точка спавна
        float spawnX = _config.GetFloat("spawn_x", -269.0f);
        float spawnY = _config.GetFloat("spawn_y", -955.0f);
        float spawnZ = _config.GetFloat("spawn_z", 31.0f);

        World.SetSyncedData("spawn_pos_x", spawnX);
        World.SetSyncedData("spawn_pos_y", spawnY);
        World.SetSyncedData("spawn_pos_z", spawnZ);

        _log.Success("Инициализация завершена.");

        // Приветствие при старте
        Chat.SendInfoToAll(welcomeMsg);
    }

    [Command("reloadconfig")]
    public void ReloadConfig(Client client)
    {
        var player = WrapPlayer(client);
        _config.Reload();
        player.SendSuccess("Настройки перезагружены. Параметров: " + _config.Count);
        _log.Info("Настройки перезагружены администратором {0}.", player.Name);
    }

    [Command("getconfig")]
    public void GetConfig(Client client, string key)
    {
        var player = WrapPlayer(client);

        if (!_config.HasKey(key))
        {
            player.SendError("Ключ '" + key + "' не найден в config.ini");
            return;
        }

        string value = _config.Get(key);
        player.SendMessage("~y~" + key + " ~w~= ~g~" + value);
    }
}
```

---

## Частые ошибки

### Ошибка 1 — создал Config каждый раз заново

Неправильно:

```csharp
[Command("info")]
public void Info(Client client)
{
    var config = new Config("config.ini");   // каждый раз читает файл
    string msg = config.Get("welcome_message");
}
```

Правильно — один раз в OnStart:

```csharp
private Config _config;

protected override void OnStart()
{
    _config = new Config("config.ini");
}

[Command("info")]
public void Info(Client client)
{
    string msg = _config.Get("welcome_message");
}
```

---

### Ошибка 2 — использовал запятую вместо точки в дробных числах

Неправильно в `config.ini`:

```ini
spawn_x=-269,0
```

Правильно:

```ini
spawn_x=-269.0
```

---

### Ошибка 3 — не проверил существование файла

Если файла нет — `Config` не выбросит ошибку, просто все ключи вернут дефолты. Но лучше предупредить об этом в логах:

```csharp
_config = new Config("config.ini");

if (!_config.FileExists)
{
    _log.Warning("Файл config.ini не найден. Работаем с дефолтными значениями.");
}
```

---

### Ошибка 4 — ожидал bool там где написано неправильное значение

Неправильно в `config.ini`:

```ini
pvp_enabled=да
```

Правильно:

```ini
pvp_enabled=true
# или
pvp_enabled=yes
# или
pvp_enabled=1
```

---

### Ошибка 5 — не использовал Reload после изменения файла

Если ты изменил `config.ini` на работающем сервере — изменения не применятся сами. Нужно вызвать:

```csharp
_config.Reload();
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
- [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html)
- [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
