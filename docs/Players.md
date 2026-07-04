---
title: Players
nav_order: 3
---

# Players

Документация по работе со списком игроков в **Glubinka MP SDK**.

`Players` — это статический класс, который даёт удобный доступ ко всем игрокам на сервере. Поиск, подсчёт, перебор — всё через него.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Важно

`Players` — статический класс. Его не нужно создавать через `new`.

Он доступен сразу внутри любого ресурса, унаследованного от `GlubinkaContext`.

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        int count = Players.Count;
        Log("Игроков на сервере: " + count);
    }
}
```

> Использовать `Players` вне `GlubinkaContext` нельзя — он не будет инициализирован.

---

## Количество игроков

```csharp
int count = Players.Count;
```

### Проверить, пуст ли сервер

```csharp
bool empty = Players.IsEmpty;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Count` | `int` | Количество игроков на сервере |
| `IsEmpty` | `bool` | Пуст ли сервер |

### Пример

```csharp
[Command("online")]
public void Online(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("~g~Онлайн: " + Players.Count + " игроков.");
}
```

---

## Получить всех игроков

### Список `List<Player>`

```csharp
List<Player> all = Players.GetAll();
```

### Массив `Player[]`

```csharp
Player[] all = Players.All;
```

| Метод / Свойство | Возвращает | Описание |
|---|---|---|
| `GetAll()` | `List<Player>` | Список всех игроков |
| `All` | `Player[]` | Массив всех игроков |

### Пример — перебрать всех игроков

```csharp
[Command("healall")]
public void HealAll(Client client)
{
    foreach (var p in Players.GetAll())
    {
        p.SetHealth(100);
        p.SetArmor(100);
        p.SendSuccess("Все игроки вылечены.");
    }

    var player = WrapPlayer(client);
    player.SendSuccess("Все игроки вылечены.");
}
```

---

## Получить имена всех игроков

```csharp
string[] names = Players.GetNames();
```

### Пример

```csharp
[Command("listplayers")]
public void ListPlayers(Client client)
{
    var player = WrapPlayer(client);
    string[] names = Players.GetNames();

    player.SendMessage("~y~Игроки на сервере:");

    foreach (string name in names)
    {
        player.SendMessage("~w~ - " + name);
    }
}
```

---

## Поиск по имени

### Точное совпадение

```csharp
Player found = Players.GetByName("Ivan");
```

Если игрок не найден — вернёт `null`.

### Без учёта регистра

```csharp
Player found = Players.GetByNameInsensitive("ivan");
```

Найдёт игрока с именем `Ivan`, `IVAN`, `ivan` и т.п.

| Метод | Описание |
|---|---|
| `GetByName(string)` | Найти по точному имени |
| `GetByNameInsensitive(string)` | Найти без учёта регистра |

### Пример

```csharp
[Command("find")]
public void Find(Client client, string targetName)
{
    var player = WrapPlayer(client);
    var target = Players.GetByNameInsensitive(targetName);

    if (target == null)
    {
        player.SendError("Игрок не найден.");
        return;
    }

    player.SendMessage("~g~Найден: " + target.Name);
}
```

---

## Проверить существование игрока

### Точное имя

```csharp
bool exists = Players.Exists("Ivan");
```

### Без учёта регистра

```csharp
bool exists = Players.ExistsInsensitive("ivan");
```

| Метод | Описание |
|---|---|
| `Exists(string)` | Есть ли игрок с таким именем (точно) |
| `ExistsInsensitive(string)` | Есть ли игрок с таким именем (без учёта регистра) |

### Пример

```csharp
[Command("isonline")]
public void IsOnline(Client client, string targetName)
{
    var player = WrapPlayer(client);

    if (Players.ExistsInsensitive(targetName))
        player.SendSuccess(targetName + " сейчас онлайн.");
    else
        player.SendError(targetName + " не в сети.");
}
```

---

## Try API — безопасный поиск

`TryGetByName` — удобный способ найти игрока без проверки на `null` вручную.

```csharp
if (Players.TryGetByName("Ivan", out Player target))
{
    target.SendMessage("~g~Тебя нашли!");
}
```

Если игрок найден — метод вернёт `true` и заполнит `target`.
Если не найден — вернёт `false`, `target` будет `null`.

| Метод | Описание |
|---|---|
| `TryGetByName(string, out Player)` | Попытаться найти игрока по имени |

### Пример

```csharp
[Command("tp")]
public void TeleportTo(Client client, string targetName)
{
    var player = WrapPlayer(client);

    if (!Players.TryGetByName(targetName, out Player target))
    {
        player.SendError("Игрок не найден.");
        return;
    }

    player.Teleport(target.Position);
    player.SendMessage("~y~Ты телепортирован к " + target.Name);
}
```

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class PlayersDemo : GlubinkaContext
{
    [Command("online")]
    public void Online(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Онлайн: " + Players.Count + " игроков.");
    }

    [Command("healall")]
    public void HealAll(Client client)
    {
        foreach (var p in Players.GetAll())
        {
            p.SetHealth(100);
            p.SetArmor(100);
            p.SendSuccess("Сервер восстановил твоё здоровье.");
        }

        var player = WrapPlayer(client);
        player.SendSuccess("Все игроки вылечены.");
    }

    [Command("find")]
    public void Find(Client client, string targetName)
    {
        var player = WrapPlayer(client);
        var target = Players.GetByNameInsensitive(targetName);

        if (target == null)
        {
            player.SendError("Игрок не найден.");
            return;
        }

        player.SendMessage("~g~Найден: " + target.Name);
        player.SendMessage("~w~Позиция: " + target.Position);
    }

    [Command("tp")]
    public void TeleportTo(Client client, string targetName)
    {
        var player = WrapPlayer(client);

        if (!Players.TryGetByName(targetName, out Player target))
        {
            player.SendError("Игрок не найден.");
            return;
        }

        player.Teleport(target.Position);
        player.SendMessage("~y~Ты телепортирован к " + target.Name);
    }

    [Command("listplayers")]
    public void ListPlayers(Client client)
    {
        var player = WrapPlayer(client);
        string[] names = Players.GetNames();

        player.SendMessage("~y~Игроки на сервере (" + Players.Count + "):");

        foreach (string name in names)
        {
            player.SendMessage("~w~ - " + name);
        }
    }
}
```

---

## Частые ошибки

### Ошибка 1 — не проверил на null после GetByName

Неправильно:

```csharp
var target = Players.GetByName("Ivan");
target.SendMessage("Привет!");
```

Правильно:

```csharp
var target = Players.GetByName("Ivan");

if (target == null)
{
    return;
}

target.SendMessage("Привет!");
```

---

### Ошибка 2 — использовал Players вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void DoSomething()
    {
        int count = Players.Count;
    }
}
```

Правильно:

```csharp
public class SomeResource : GlubinkaContext
{
    protected override void OnStart()
    {
        int count = Players.Count;
    }
}
```

> `Players` инициализируется только внутри `GlubinkaContext`. Вне него он выбросит ошибку.

---

### Ошибка 3 — забыл, что GetByName чувствителен к регистру

Неправильно — игрок с именем `Ivan` не будет найден:

```csharp
var target = Players.GetByName("ivan");
```

Правильно — использовать insensitive поиск:

```csharp
var target = Players.GetByNameInsensitive("ivan");
```

---

### Ошибка 4 — передал пустую строку в поиск

Неправильно:

```csharp
var target = Players.GetByName("");
```

Правильно:

```csharp
if (!string.IsNullOrWhiteSpace(targetName))
{
    var target = Players.GetByName(targetName);
}
```

> При пустой строке `GetByName` и `GetByNameInsensitive` сразу вернут `null`.

---

## Все методы Players — сводная таблица

| Метод / Свойство | Возвращает | Описание |
|---|---|---|
| `Count` | `int` | Количество игроков на сервере |
| `IsEmpty` | `bool` | Пуст ли сервер |
| `All` | `Player[]` | Все игроки в виде массива |
| `GetAll()` | `List<Player>` | Все игроки в виде списка |
| `GetNames()` | `string[]` | Имена всех игроков |
| `GetByName(string)` | `Player` | Найти игрока по точному имени |
| `GetByNameInsensitive(string)` | `Player` | Найти игрока без учёта регистра |
| `Exists(string)` | `bool` | Есть ли игрок с таким именем (точно) |
| `ExistsInsensitive(string)` | `bool` | Есть ли игрок с таким именем (без регистра) |
| `TryGetByName(string, out Player)` | `bool` | Безопасный поиск игрока по имени |

---

## См. также

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
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
- [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
