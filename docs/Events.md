# Events

Документация по серверным событиям в **Glubinka MP SDK**.

События — это функции, которые вызываются ядром автоматически при определённых действиях: подключение игрока, смерть, вход в транспорт, выстрел и т.п. Ты подписываешься на событие и пишешь свою логику.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как подписаться на событие

Подписывайся на события в `OnStart`. Отписывайся в `OnStop`.

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        API.onPlayerConnected += OnPlayerConnected;
        API.onPlayerDisconnected += OnPlayerDisconnected;
    }

    protected override void OnStop()
    {
        API.onPlayerConnected -= OnPlayerConnected;
        API.onPlayerDisconnected -= OnPlayerDisconnected;
    }

    private void OnPlayerConnected(Client client)
    {
        var player = WrapPlayer(client);
        player.SendSuccess("Добро пожаловать на сервер!");
    }

    private void OnPlayerDisconnected(Client client, string reason)
    {
        Log("Игрок " + client.Name + " отключился: " + reason);
    }
}
```

> Не подписывайся на события внутри команд или других методов — только в `OnStart`.

---

## События ресурса

### onResourceStart

Вызывается когда ресурс запускается.

```csharp
API.onResourceStart += OnResourceStart;
```

```csharp
private void OnResourceStart()
{
    Log("Ресурс запущен.");
}
```

> В SDK удобнее использовать `OnStart()` из `GlubinkaContext` — он делает то же самое.

---

### onResourceStop

Вызывается когда ресурс останавливается.

```csharp
API.onResourceStop += OnResourceStop;
```

```csharp
private void OnResourceStop()
{
    Log("Ресурс остановлен.");
}
```

> В SDK удобнее использовать `OnStop()` из `GlubinkaContext`.

---

## События игроков

### onPlayerConnected

Вызывается когда игрок подключается к серверу.

```csharp
API.onPlayerConnected += OnPlayerConnected;
```

```csharp
private void OnPlayerConnected(Client client)
{
    var player = WrapPlayer(client);
    player.SendSuccess("Добро пожаловать, " + player.Name + "!");
    Chat.SendInfoToAll(player.Name + " подключился к серверу.");
}
```

---

### onPlayerDisconnected

Вызывается когда игрок отключается от сервера.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `reason` | `string` | Причина отключения |

```csharp
API.onPlayerDisconnected += OnPlayerDisconnected;
```

```csharp
private void OnPlayerDisconnected(Client client, string reason)
{
    Chat.SendWarningToAll(client.Name + " покинул сервер: " + reason);
}
```

---

### onPlayerDeath

Вызывается когда игрок умирает.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Погибший игрок |
| `killer` | `NetHandle` | Handle убийцы (или пустой) |
| `weapon` | `int` | Хэш оружия |

```csharp
API.onPlayerDeath += OnPlayerDeath;
```

```csharp
private void OnPlayerDeath(Client client, NetHandle killer, int weapon)
{
    var player = WrapPlayer(client);
    Chat.SendErrorToAll(player.Name + " погиб.");
}
```

---

### onPlayerRespawn

Вызывается когда игрок респавнится после смерти.

```csharp
API.onPlayerRespawn += OnPlayerRespawn;
```

```csharp
private void OnPlayerRespawn(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
    player.Teleport(-269.0f, -955.0f, 31.0f);
    player.SendMessage("~g~Ты возродился.");
}
```

---

### onPlayerHealthChange

Вызывается когда у игрока меняется здоровье.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `oldValue` | `int` | Предыдущее значение |

```csharp
API.onPlayerHealthChange += OnPlayerHealthChange;
```

```csharp
private void OnPlayerHealthChange(Client client, int oldValue)
{
    var player = WrapPlayer(client);

    if (player.Health < 20)
    {
        player.SendWarning("Осторожно! Здоровье критически низкое!");
    }
}
```

---

### onPlayerArmorChange

Вызывается когда у игрока меняется броня.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `oldValue` | `int` | Предыдущее значение |

```csharp
API.onPlayerArmorChange += OnPlayerArmorChange;
```

```csharp
private void OnPlayerArmorChange(Client client, int oldValue)
{
    var player = WrapPlayer(client);
    Log(player.Name + " — броня изменилась: " + oldValue + " → " + player.Armor);
}
```

---

### onPlayerWeaponSwitch

Вызывается когда игрок переключает оружие.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `oldWeapon` | `WeaponHash` | Предыдущее оружие |
| `newWeapon` | `WeaponHash` | Новое оружие |

```csharp
API.onPlayerWeaponSwitch += OnPlayerWeaponSwitch;
```

```csharp
private void OnPlayerWeaponSwitch(Client client, WeaponHash oldWeapon, WeaponHash newWeapon)
{
    var player = WrapPlayer(client);
    Log(player.Name + " сменил оружие: " + oldWeapon + " → " + newWeapon);
}
```

---

### onPlayerModelChange

Вызывается когда меняется модель (скин) игрока.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `oldModel` | `int` | Предыдущий хэш модели |

```csharp
API.onPlayerModelChange += OnPlayerModelChange;
```

```csharp
private void OnPlayerModelChange(Client client, int oldModel)
{
    var player = WrapPlayer(client);
    Log(player.Name + " сменил скин.");
}
```

---

## События чата

### onChatMessage

Вызывается когда игрок отправляет сообщение в чат.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `message` | `string` | Текст сообщения |
| `cancel` | `CancelEventArgs` | Можно отменить отправку |

```csharp
API.onChatMessage += OnChatMessage;
```

```csharp
private void OnChatMessage(Client client, string message, CancelEventArgs cancel)
{
    var player = WrapPlayer(client);

    // Запретить слово
    if (message.ToLower().Contains("badword"))
    {
        cancel.Cancel = true;
        player.SendError("Это слово запрещено.");
        return;
    }

    Log("[ЧАТ] " + player.Name + ": " + message);
}
```

---

### onChatCommand

Вызывается когда игрок вводит любую команду (до обработки `[Command]`).

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `command` | `string` | Полная строка команды |
| `cancel` | `CancelEventArgs` | Можно отменить выполнение |

```csharp
API.onChatCommand += OnChatCommand;
```

```csharp
private void OnChatCommand(Client client, string command, CancelEventArgs cancel)
{
    var player = WrapPlayer(client);
    Log("[CMD] " + player.Name + ": " + command);
}
```

---

## События транспорта

### onPlayerEnterVehicle

Вызывается когда игрок садится в транспорт.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `vehicle` | `NetHandle` | Handle транспорта |
| `targetSeat` | `int` | Сиденье (-1 = водитель) |

```csharp
API.onPlayerEnterVehicle += OnPlayerEnterVehicle;
```

```csharp
private void OnPlayerEnterVehicle(Client client, NetHandle vehicle, int targetSeat)
{
    var player = WrapPlayer(client);
    player.SendMessage("~y~Ты сел в транспорт.");
}
```

---

### onPlayerExitVehicle

Вызывается когда игрок выходит из транспорта.

| Параметр | Тип | Описание |
|---|---|---|
| `client` | `Client` | Игрок |
| `vehicle` | `NetHandle` | Handle транспорта |

```csharp
API.onPlayerExitVehicle += OnPlayerExitVehicle;
```

```csharp
private void OnPlayerExitVehicle(Client client, NetHandle vehicle)
{
    var player = WrapPlayer(client);
    player.SendMessage("~y~Ты вышел из транспорта.");
}
```

---

### onVehicleDeath

Вызывается когда транспорт уничтожен.

| Параметр | Тип | Описание |
|---|---|---|
| `vehicle` | `NetHandle` | Handle транспорта |

```csharp
API.onVehicleDeath += OnVehicleDeath;
```

```csharp
private void OnVehicleDeath(NetHandle vehicle)
{
    Log("Транспорт уничтожен.");
}
```

---

## События колшейпов

### onEntityEnterColShape

Вызывается когда игрок или транспорт входит в колшейп.

| Параметр | Тип | Описание |
|---|---|---|
| `shape` | `ColShape` | Внутренний колшейп ядра |
| `entity` | `NetHandle` | Handle сущности |

```csharp
API.onEntityEnterColShape += OnEntityEnterColShape;
```

```csharp
private void OnEntityEnterColShape(ColShape shape, NetHandle entity)
{
    if (!API.isPlayer(entity))
        return;

    var client = API.getPlayerFromHandle(entity);
    if (client == null)
        return;

    var player = WrapPlayer(client);

    if (_myZone.IsSame(shape))
    {
        player.SendMessage("~g~Ты вошёл в зону.");
    }
}
```

---

### onEntityExitColShape

Вызывается когда игрок или транспорт выходит из колшейпа.

| Параметр | Тип | Описание |
|---|---|---|
| `shape` | `ColShape` | Внутренний колшейп ядра |
| `entity` | `NetHandle` | Handle сущности |

```csharp
API.onEntityExitColShape += OnEntityExitColShape;
```

```csharp
private void OnEntityExitColShape(ColShape shape, NetHandle entity)
{
    if (!API.isPlayer(entity))
        return;

    var client = API.getPlayerFromHandle(entity);
    if (client == null)
        return;

    var player = WrapPlayer(client);

    if (_myZone.IsSame(shape))
    {
        player.SendMessage("~y~Ты вышел из зоны.");
    }
}
```

> Подробнее о работе с колшейпами — в разделе [ColShape](https://glubinkamp.github.io/GlubinkaMP-Docs/ColShape.html).

---

## События обновления

### onUpdate

Вызывается каждый серверный тик. Используй осторожно — вызывается очень часто.

```csharp
API.onUpdate += OnUpdate;
```

```csharp
private void OnUpdate()
{
    // выполняется каждый тик — используй с умом
}
```

> ⚠️ Не делай тяжёлых операций внутри `onUpdate`. Это может сильно повлиять на производительность сервера.

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class EventsDemo : GlubinkaContext
{
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("EventsDemo");

        API.onPlayerConnected += OnPlayerConnected;
        API.onPlayerDisconnected += OnPlayerDisconnected;
        API.onPlayerDeath += OnPlayerDeath;
        API.onPlayerRespawn += OnPlayerRespawn;
        API.onPlayerEnterVehicle += OnPlayerEnterVehicle;
        API.onPlayerExitVehicle += OnPlayerExitVehicle;
        API.onChatMessage += OnChatMessage;

        _log.Success("Ресурс запущен.");
    }

    protected override void OnStop()
    {
        API.onPlayerConnected -= OnPlayerConnected;
        API.onPlayerDisconnected -= OnPlayerDisconnected;
        API.onPlayerDeath -= OnPlayerDeath;
        API.onPlayerRespawn -= OnPlayerRespawn;
        API.onPlayerEnterVehicle -= OnPlayerEnterVehicle;
        API.onPlayerExitVehicle -= OnPlayerExitVehicle;
        API.onChatMessage -= OnChatMessage;

        _log.Info("Ресурс остановлен.");
    }

    private void OnPlayerConnected(Client client)
    {
        var player = WrapPlayer(client);
        player.SendSuccess("Добро пожаловать, " + player.Name + "!");
        Chat.SendInfoToAll(player.Name + " подключился.");
        _log.Info("Игрок {0} подключился.", player.Name);
    }

    private void OnPlayerDisconnected(Client client, string reason)
    {
        Chat.SendWarningToAll(client.Name + " покинул сервер.");
        _log.Info("Игрок {0} отключился: {1}", client.Name, reason);
    }

    private void OnPlayerDeath(Client client, NetHandle killer, int weapon)
    {
        var player = WrapPlayer(client);
        Chat.SendErrorToAll(player.Name + " погиб.");
        _log.Info("Игрок {0} погиб.", player.Name);
    }

    private void OnPlayerRespawn(Client client)
    {
        var player = WrapPlayer(client);
        player.SetHealth(100);
        player.SetArmor(0);
        player.Teleport(-269.0f, -955.0f, 31.0f);
        player.SendMessage("~g~Ты возродился.");
    }

    private void OnPlayerEnterVehicle(Client client, NetHandle vehicle, int targetSeat)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~y~Ты сел в транспорт.");
    }

    private void OnPlayerExitVehicle(Client client, NetHandle vehicle)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~y~Ты вышел из транспорта.");
    }

    private void OnChatMessage(Client client, string message, CancelEventArgs cancel)
    {
        var player = WrapPlayer(client);
        _log.Info("[ЧАТ] {0}: {1}", player.Name, message);
    }
}
```

---

## Частые ошибки

### Ошибка 1 — подписался на событие внутри команды

Неправильно:

```csharp
[Command("start")]
public void Start(Client client)
{
    API.onPlayerConnected += OnPlayerConnected;   // каждый вызов добавляет ещё одну подписку
}
```

Правильно — только в `OnStart`:

```csharp
protected override void OnStart()
{
    API.onPlayerConnected += OnPlayerConnected;
}
```

---

### Ошибка 2 — забыл отписаться от события

Неправильно:

```csharp
protected override void OnStart()
{
    API.onPlayerConnected += OnPlayerConnected;
}
// OnStop не переопределён
```

Правильно:

```csharp
protected override void OnStop()
{
    API.onPlayerConnected -= OnPlayerConnected;
}
```

---

### Ошибка 3 — не проверил тип сущности в колшейпе

Неправильно:

```csharp
private void OnEntityEnterColShape(ColShape shape, NetHandle entity)
{
    var client = API.getPlayerFromHandle(entity);   // упадёт если entity — транспорт
    var player = WrapPlayer(client);
}
```

Правильно:

```csharp
private void OnEntityEnterColShape(ColShape shape, NetHandle entity)
{
    if (!API.isPlayer(entity))
        return;

    var client = API.getPlayerFromHandle(entity);
    if (client == null)
        return;

    var player = WrapPlayer(client);
}
```

---

### Ошибка 4 — забыл WrapPlayer

Неправильно:

```csharp
private void OnPlayerConnected(Client client)
{
    client.SendMessage("Привет!");   // нет такого метода у Client
}
```

Правильно:

```csharp
private void OnPlayerConnected(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("Привет!");
}
```

---

## Все события — сводная таблица

| Событие | Когда вызывается |
|---|---|
| `onResourceStart` | Ресурс запущен |
| `onResourceStop` | Ресурс остановлен |
| `onPlayerConnected` | Игрок подключился |
| `onPlayerDisconnected` | Игрок отключился |
| `onPlayerDeath` | Игрок погиб |
| `onPlayerRespawn` | Игрок возродился |
| `onPlayerHealthChange` | Изменилось здоровье |
| `onPlayerArmorChange` | Изменилась броня |
| `onPlayerWeaponSwitch` | Сменил оружие |
| `onPlayerModelChange` | Сменился скин |
| `onChatMessage` | Сообщение в чат |
| `onChatCommand` | Введена команда |
| `onPlayerEnterVehicle` | Сел в транспорт |
| `onPlayerExitVehicle` | Вышел из транспорта |
| `onVehicleDeath` | Транспорт уничтожен |
| `onEntityEnterColShape` | Вход в колшейп |
| `onEntityExitColShape` | Выход из колшейпа |
| `onUpdate` | Каждый серверный тик |

---

## См. также
