
# ⌨️ Commands

Документация по созданию серверных команд в **Glubinka MP**.

Команды — это самый простой способ дать игроку возможность вызвать твой код через чат.  
#### Например:
```
- `/help`
- `/heal`
- `/car adder`
- `/weather 1`
```
---

## Как работает команда

#### Игрок пишет команду в чат, например:

```
/heal 100
```

#### Сервер находит метод с атрибутом:

```
[Command("heal")]
```

#### и вызывает его.

---

## Самое важное правило

### В атрибуте команда пишется **без слэша**
#### Правильно:

```
[Command("heal")]
```

#### Неправильно:

```
[Command("/heal")]
```

#### Игрок в игре всё равно пишет команду **со слэшем**:

```
/heal
```

---

## Минимальный пример команды

```
using GTANetworkServer;
using GTANetworkShared;

public class MyCommands : Script
{
    [Command("hello")]
    public void HelloCommand(Client player)
    {
        API.sendChatMessageToPlayer(player, "~g~Привет!");
    }
}
```

#### Игрок пишет в чат:

```
/hello
```

#### И получает сообщение.

---

## Базовая структура

#### Обычно команда выглядит так:

```csharp
[Command("имя_команды")]
public void MyCommand(Client player)
{
    // твой код
}
```

#### Где:
```
- `[Command("имя_команды")]` — имя команды
- `Client player` — игрок, который ввёл команду
```
---

## Команда без параметров

```
[Command("gpos")]
public void GetPositionCommand(Client player)
{
    Vector3 pos = player.Position;
    API.sendChatMessageToPlayer(
        player,
        "~y~Твоя позиция: ~w~X: " + pos.X + " Y: " + pos.Y + " Z: " + pos.Z
    );
}
```

#### Игрок использует:

```text
/gpos
```

---

## Команда с числовым параметром

```
[Command("gheal")]
public void HealCommand(Client player, int amount)
{
    if (amount < 1) amount = 1;
    if (amount > 100) amount = 100;

    API.setPlayerHealth(player, amount);
    API.sendChatMessageToPlayer(player, "~g~Здоровье установлено: ~w~" + amount);
}
```

#### Игрок использует:

```
/gheal 100
```

---

## Команда с несколькими параметрами

```
[Command("gweather")]
public void WeatherCommand(Client player, int weatherId)
{
    if (weatherId < 0 || weatherId > 13)
    {
        API.sendChatMessageToPlayer(player, "~r~Погода должна быть в диапазоне 0-13.");
        return;
    }

    API.setWeather(weatherId);
    API.sendChatMessageToAll("~b~Погода изменена на ID: ~w~" + weatherId);
}
```

#### Игрок использует:

```
/gweather 1
```

### ⚠️ Для погоды используй значения **0-13**.  
### Значения вне диапазона могут вызывать проблемы.

---

## Команда со строковым параметром

```
[Command("sayhi")]
public void SayHiCommand(Client player, string name)
{
    API.sendChatMessageToAll("~g~" + player.Name + " приветствует " + name + "!");
}
```

#### Игрок использует:

```
/sayhi Ivan
```

---

## Пример команды спавна транспорта

```
[Command("gcar")]
public void SpawnCarCommand(Client player, string vehicleName)
{
    VehicleHash model = API.vehicleNameToModel(vehicleName);

    if ((int)model == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестная модель транспорта.");
        return;
    }

    Vector3 spawnPos = new Vector3(
        player.Position.X + 2.0f,
        player.Position.Y,
        player.Position.Z
    );

    API.createVehicle(model, spawnPos, new Vector3(0, 0, 0), 0, 0);
    API.sendChatMessageToPlayer(player, "~g~Транспорт создан: ~w~" + vehicleName);
}
```

#### Игрок использует:

```
/gcar adder
```

---

## Пример команды убийства игрока самого себя

```
[Command("gkill")]
public void KillCommand(Client player)
{
    API.setPlayerHealth(player, 0);
    API.sendChatMessageToPlayer(player, "~r~Ты умер.");
}
```

#### Игрок использует:

```
/gkill
```

---

## Пример команды времени

```
[Command("gtime")]
public void TimeCommand(Client player, int hours, int minutes)
{
    if (hours < 0 || hours > 23)
    {
        API.sendChatMessageToPlayer(player, "~r~Часы должны быть от 0 до 23.");
        return;
    }

    if (minutes < 0 || minutes > 59)
    {
        API.sendChatMessageToPlayer(player, "~r~Минуты должны быть от 0 до 59.");
        return;
    }

    API.setTime(hours, minutes);
    API.sendChatMessageToAll("~y~Время изменено на ~w~" + hours + ":" + minutes);
}
```

#### Игрок использует:

```
/gtime 12 30
```

---

## Набор простых рабочих команд в одном ресурсе

```
using GTANetworkServer;
using GTANetworkShared;

public class BasicCommands : Script
{
    [Command("ghelp")]
    public void HelpCommand(Client player)
    {
        API.sendChatMessageToPlayer(player, "~y~/gpos ~w~- показать позицию");
        API.sendChatMessageToPlayer(player, "~y~/gheal [1-100] ~w~- вылечить себя");
        API.sendChatMessageToPlayer(player, "~y~/gkill ~w~- убить себя");
        API.sendChatMessageToPlayer(player, "~y~/gweather [0-13] ~w~- сменить погоду");
        API.sendChatMessageToPlayer(player, "~y~/gtime [часы] [минуты] ~w~- сменить время");
        API.sendChatMessageToPlayer(player, "~y~/gcar [model] ~w~- создать транспорт");
    }

    [Command("gpos")]
    public void PositionCommand(Client player)
    {
        Vector3 pos = player.Position;
        API.sendChatMessageToPlayer(
            player,
            "~g~X: ~w~" + pos.X + " ~g~Y: ~w~" + pos.Y + " ~g~Z: ~w~" + pos.Z
        );
    }

    [Command("gheal")]
    public void HealCommand(Client player, int amount)
    {
        if (amount < 1 || amount > 100)
        {
            API.sendChatMessageToPlayer(player, "~r~Используй число от 1 до 100.");
            return;
        }

        API.setPlayerHealth(player, amount);
        API.sendChatMessageToPlayer(player, "~g~Здоровье установлено: ~w~" + amount);
    }

    [Command("gkill")]
    public void KillCommand(Client player)
    {
        API.setPlayerHealth(player, 0);
        API.sendChatMessageToPlayer(player, "~r~Ты убил себя.");
    }

    [Command("gweather")]
    public void WeatherCommand(Client player, int weatherId)
    {
        if (weatherId < 0 || weatherId > 13)
        {
            API.sendChatMessageToPlayer(player, "~r~Погода должна быть от 0 до 13.");
            return;
        }

        API.setWeather(weatherId);
        API.sendChatMessageToAll("~b~Погода изменена: ~w~" + weatherId);
    }

    [Command("gtime")]
    public void TimeCommand(Client player, int hours, int minutes)
    {
        if (hours < 0 || hours > 23 || minutes < 0 || minutes > 59)
        {
            API.sendChatMessageToPlayer(player, "~r~Пример: /gtime 12 30");
            return;
        }

        API.setTime(hours, minutes);
        API.sendChatMessageToAll("~y~Новое время: ~w~" + hours + ":" + minutes);
    }

    [Command("gcar")]
    public void CarCommand(Client player, string vehicleName)
    {
        VehicleHash model = API.vehicleNameToModel(vehicleName);

        if ((int)model == 0)
        {
            API.sendChatMessageToPlayer(player, "~r~Неизвестная модель транспорта.");
            return;
        }

        Vector3 spawnPos = new Vector3(
            player.Position.X + 2.0f,
            player.Position.Y,
            player.Position.Z
        );

        API.createVehicle(model, spawnPos, new Vector3(0, 0, 0), 0, 0);
        API.sendChatMessageToPlayer(player, "~g~Создан транспорт: ~w~" + vehicleName);
    }
}
```

---

## Какие параметры использовать

#### Для базовых команд безопаснее всего использовать:
```
- `Client`
- `string`
- `int`
- `float`
- `bool`
```
#### Примеры:

```
[Command("test1")]
public void Test1(Client player)
{
}
```

```
[Command("test2")]
public void Test2(Client player, int value)
{
}
```

```
[Command("test3")]
public void Test3(Client player, string text)
{
}
```

```
[Command("test4")]
public void Test4(Client player, float value)
{
}
```

```
[Command("test5")]
public void Test5(Client player, bool enabled)
{
}
```

---

## Что делать, если нужны сложные аргументы

#### Если тебе нужно:
```
- много слов после команды
- нестандартный синтаксис
- собственный парсер
- полная строка команды целиком
```
#### тогда лучше использовать событие:

```
API.onChatCommand += OnChatCommand;
```

#### Пример:

```
using GTANetworkServer;

public class CustomCommandParser : Script
{
    public CustomCommandParser()
    {
        API.onChatCommand += OnChatCommand;
    }

    private void OnChatCommand(Client player, string command, CancelEventArgs cancel)
    {
        if (!command.StartsWith("/announce ")) return;

        cancel.Cancel = true;

        string text = command.Substring("/announce ".Length);
        if (string.IsNullOrWhiteSpace(text))
        {
            API.sendChatMessageToPlayer(player, "~r~Использование: /announce [текст]");
            return;
        }

        API.sendChatMessageToAll("~y~[ОБЪЯВЛЕНИЕ] ~w~" + text);
    }
}
```

#### Игрок использует:

```
/announce Сервер будет перезапущен через 5 минут
```

---

## Разница между `[Command]` и `onChatCommand`

### `[Command("name")]`
#### Используй, если:
```
- команда простая
- известное количество параметров
- нужен самый удобный вариант
```
### `API.onChatCommand`
#### Используй, если:
```
- нужна полная строка команды
- нужен свой разбор аргументов
- нужен контроль над всеми командами
- нужно отменять команды вручную
```
---

## Как показать игроку правильный синтаксис

#### Лучше сразу писать игроку пример использования.

### Плохой вариант:
```
API.sendChatMessageToPlayer(player, "~r~Ошибка.");
```

### Хороший вариант:
```csharp
API.sendChatMessageToPlayer(player, "~r~Использование: /gtime [часы] [минуты]");
```

---

## Как изменить стандартное сообщение об ошибке команды

### API сервера позволяет задать своё сообщение об ошибке:

```
public MyResource()
{
    API.onResourceStart += OnResourceStart;
}

private void OnResourceStart()
{
    API.setCommandErrorMessage("Неверный синтаксис команды.");
}
```

### Это полезно, если ты хочешь, чтобы игроки видели понятное сообщение вместо стандартного.

---

## Частые ошибки

### Ошибка 1 — добавлен слэш в атрибуте

#### Неправильно:
```
[Command("/heal")]
```

#### Правильно:
```
[Command("heal")]
```

---

### Ошибка 2 — забыли параметр `Client player`

#### Неправильно:
```
[Command("heal")]
public void HealCommand(int amount)
{
}
```

#### Правильно:
```
[Command("heal")]
public void HealCommand(Client player, int amount)
{
}
```

---

### Ошибка 3 — нет проверки диапазона

#### Неправильно:
```
[Command("gweather")]
public void WeatherCommand(Client player, int weatherId)
{
    API.setWeather(weatherId);
}
```

#### Правильно:
```
[Command("gweather")]
public void WeatherCommand(Client player, int weatherId)
{
    if (weatherId < 0 || weatherId > 13)
    {
        API.sendChatMessageToPlayer(player, "~r~Погода должна быть от 0 до 13.");
        return;
    }

    API.setWeather(weatherId);
}
```

---

### Ошибка 4 — нет проверки модели транспорта

#### Неправильно:
```
[Command("gcar")]
public void CarCommand(Client player, string vehicleName)
{
    VehicleHash model = API.vehicleNameToModel(vehicleName);
    API.createVehicle(model, player.Position, new Vector3(0,0,0), 0, 0);
}
```

#### Правильно:
```
[Command("gcar")]
public void CarCommand(Client player, string vehicleName)
{
    VehicleHash model = API.vehicleNameToModel(vehicleName);

    if ((int)model == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестная модель транспорта.");
        return;
    }

    API.createVehicle(model, player.Position, new Vector3(0,0,0), 0, 0);
}
```

---

### Ошибка 5 — слишком сложная команда через атрибут

#### Если команда становится слишком сложной, не мучай один метод с огромным количеством параметров.  
#### Проще перейти на:

```
API.onChatCommand += OnChatCommand;
```

#### и разобрать строку вручную.

---

# Советы

### 1. Делай короткие понятные имена
#### Хорошо:
```
- `/help`
- `/car`
- `/heal`
- `/time`
```
#### Плохо:
```
- `/superultracommand123`
```
---

### 2. Всегда проверяй пользовательский ввод
#### Игрок может ввести:
```
- отрицательное число
- слишком большое число
- несуществующую модель
- пустой текст
```
---

### 3. Показывай игроку результат
#### После команды хорошо отправлять подтверждение:

```
API.sendChatMessageToPlayer(player, "~g~Команда выполнена.");
```

---

### 4. Не делай опасные команды без ограничений
#### Команды вроде:
```
- спавна транспорта
- выдачи оружия
- бана
- смены мира
```
#### лучше ограничивать по правам, админ-статусу или whitelist.

---

## Пример простой админ-команды

```
[Command("sethp")]
public void SetHpCommand(Client player, int hp)
{
    if (player.Name != "Admin")
    {
        API.sendChatMessageToPlayer(player, "~r~У тебя нет доступа к этой команде.");
        return;
    }

    if (hp < 1 || hp > 100)
    {
        API.sendChatMessageToPlayer(player, "~r~Использование: /sethp [1-100]");
        return;
    }

    API.setPlayerHealth(player, hp);
    API.sendChatMessageToPlayer(player, "~g~HP установлено: ~w~" + hp);
}
```

---

## См. также

- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
```
