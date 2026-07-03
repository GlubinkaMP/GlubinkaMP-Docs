Понял. Пишу все три файла по очереди.

---

# Commands

Документация по созданию серверных команд в **Glubinka MP SDK**.

Команды — это действия, которые игрок запускает из чата, вводя `/имякоманды`. Сервер перехватывает текст и вызывает нужный метод.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как создать команду

Команда создаётся через атрибут `[Command]` над методом.

Первый параметр метода — всегда `Client client`.

```csharp
[Command("hello")]
public void HelloCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("~g~Привет, " + player.Name + "!");
}
```

Игрок вводит в чат:

```
/hello
```

---

## Команда с параметрами

Параметры добавляются как дополнительные аргументы метода.

### Один параметр

```csharp
[Command("heal")]
public void HealCommand(Client client, int amount)
{
    var player = WrapPlayer(client);
    player.SetHealth(amount);
    player.SendSuccess("Здоровье установлено: " + amount);
}
```

```
/heal 100
```

### Несколько параметров

```csharp
[Command("tp")]
public void TeleportCommand(Client client, float x, float y, float z)
{
    var player = WrapPlayer(client);
    player.Teleport(x, y, z);
    player.SendSuccess("Ты телепортирован.");
}
```

```
/tp -269.0 -955.0 31.0
```

---

## Строковый параметр с пробелами

Если последний параметр — строка и ты хочешь, чтобы он включал пробелы, используй `GreedyArg = true`.

```csharp
[Command("say", GreedyArg = true)]
public void SayCommand(Client client, string message)
{
    var player = WrapPlayer(client);
    Chat.SendToAllFrom(player.Name, message);
}
```

```
/say Привет всем на сервере!
```

> Без `GreedyArg` слово после первого пробела будет отброшено.

---

## Команда с необязательным параметром

```csharp
[Command("kick")]
public void KickCommand(Client client, string targetName, string reason = "Не указана")
{
    var player = WrapPlayer(client);

    if (!Players.TryGetByName(targetName, out Player target))
    {
        player.SendError("Игрок не найден.");
        return;
    }

    target.Kick(reason);
    player.SendSuccess("Игрок " + target.Name + " кикнут. Причина: " + reason);
}
```

```
/kick Ivan
/kick Ivan Нарушение правил
```

---

## Типы параметров

Ядро автоматически конвертирует текст в нужный тип.

| Тип C# | Что игрок вводит | Пример |
|---|---|---|
| `string` | Текст | `/cmd hello` |
| `int` | Целое число | `/cmd 42` |
| `float` | Дробное число | `/cmd 3.14` |
| `double` | Дробное число | `/cmd 3.14159` |
| `bool` | true / false | `/cmd true` |

---

## Алиасы команд

Одна и та же логика под разными именами — просто создай два метода или используй два атрибута.

```csharp
[Command("hp")]
public void HpCommand(Client client)
{
    ShowHealth(client);
}

[Command("health")]
public void HealthCommand(Client client)
{
    ShowHealth(client);
}

private void ShowHealth(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("~g~Здоровье: " + player.Health);
}
```

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class CommandsDemo : GlubinkaContext
{
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("CommandsDemo");
        _log.Success("Команды загружены.");
    }

    [Command("hello")]
    public void HelloCommand(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Привет, " + player.Name + "!");
    }

    [Command("heal")]
    public void HealCommand(Client client, int amount)
    {
        var player = WrapPlayer(client);
        player.SetHealth(amount);
        player.SetArmor(100);
        player.SendSuccess("Здоровье: " + amount + ", броня: 100.");
        _log.Info("{0} использовал /heal {1}", player.Name, amount);
    }

    [Command("spawncar")]
    public void SpawnCarCommand(Client client, string modelName)
    {
        var player = WrapPlayer(client);
        var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
        player.PutIntoVehicle(car);
        player.SendSuccess("Машина создана.");
        _log.Info("{0} заспавнил машину.", player.Name);
    }

    [Command("tp")]
    public void TeleportCommand(Client client, float x, float y, float z)
    {
        var player = WrapPlayer(client);
        player.Teleport(x, y, z);
        player.SendSuccess("Телепортирован.");
    }

    [Command("kick")]
    public void KickCommand(Client client, string targetName, string reason = "Не указана")
    {
        var player = WrapPlayer(client);

        if (!Players.TryGetByName(targetName, out Player target))
        {
            player.SendError("Игрок не найден.");
            return;
        }

        Chat.SendWarningToAll(target.Name + " кикнут. Причина: " + reason);
        target.Kick(reason);
        _log.Info("{0} кикнул {1}. Причина: {2}", player.Name, target.Name, reason);
    }

    [Command("say", GreedyArg = true)]
    public void SayCommand(Client client, string message)
    {
        var player = WrapPlayer(client);
        Chat.SendToAllFrom(player.Name, message);
    }

    [Command("online")]
    public void OnlineCommand(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Онлайн: " + Players.Count + " игроков.");
    }

    [Command("weather")]
    public void WeatherCommand(Client client, int id)
    {
        var player = WrapPlayer(client);
        World.SetWeather(id);
        player.SendSuccess("Погода: " + World.GetWeatherName(id));
    }

    [Command("time")]
    public void TimeCommand(Client client, int hour, int minute)
    {
        var player = WrapPlayer(client);
        World.SetTime(hour, minute);
        player.SendSuccess("Время: " + hour + ":" + minute.ToString("D2"));
    }
}
```

---

## Частые ошибки

### Ошибка 1 — забыл первый параметр Client

Неправильно:

```csharp
[Command("hello")]
public void HelloCommand()
{
}
```

Правильно:

```csharp
[Command("hello")]
public void HelloCommand(Client client)
{
}
```

---

### Ошибка 2 — не обернул Client в Player

Неправильно:

```csharp
[Command("heal")]
public void HealCommand(Client client)
{
    client.SetHealth(100);   // у Client нет такого метода
}
```

Правильно:

```csharp
[Command("heal")]
public void HealCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
}
```

---

### Ошибка 3 — не указал GreedyArg для строки с пробелами

Неправильно — получишь только первое слово:

```csharp
[Command("say")]
public void SayCommand(Client client, string message)
{
    Chat.SendToAll(message);   // "Привет" вместо "Привет всем!"
}
```

Правильно:

```csharp
[Command("say", GreedyArg = true)]
public void SayCommand(Client client, string message)
{
    Chat.SendToAll(message);   // "Привет всем!"
}
```

---

### Ошибка 4 — передал неправильный тип параметра

Если команда ожидает `int`, а игрок ввёл текст — будет ошибка.

```
/heal abc
```

Для безопасности можно ловить исключения или использовать строковые параметры и парсить вручную.

---

### Ошибка 5 — создал команду вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    [Command("hello")]
    public void HelloCommand(Client client) { }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    [Command("hello")]
    public void HelloCommand(Client client) { }
}
```

---

## См. также
