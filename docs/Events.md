# 📡 Events

Документация по серверным событиям платформы **Glubinka MP**.

Все события ниже составлены по реальному серверному API.

---

## Что такое событие

Событие — это момент, когда сервер сообщает твоему ресурсу, что что-то произошло.

Например:

- подключился игрок
- игрок умер
- игрок сел в транспорт
- кто-то написал сообщение в чат
- запустился ресурс

Ты подписываешься на событие в конструкторе класса, а потом пишешь метод, который будет выполняться при срабатывании этого события.

---

## Как подписаться на событие

```csharp
using GTANetworkServer;
using GTANetworkShared;

public class MyResource : Script
{
    public MyResource()
    {
        API.onResourceStart += OnResourceStart;
        API.onPlayerConnected += OnPlayerConnected;
        API.onPlayerDisconnected += OnPlayerDisconnected;
    }

    private void OnResourceStart()
    {
        API.consoleOutput("Ресурс запущен!");
    }

    private void OnPlayerConnected(Client player)
    {
        API.sendChatMessageToAll("~g~► ~w~" + player.Name + " подключился!");
    }

    private void OnPlayerDisconnected(Client player, string reason)
    {
        API.sendChatMessageToAll("~r~◄ ~w~" + player.Name + " отключился. Причина: " + reason);
    }
}
```

## Важные правила
### 1. Подписывайся на события только в конструкторе
Правильно:

```
public MyResource()
{
    API.onPlayerConnected += OnPlayerConnected;
}
Неправильно:
```
```
private void OnResourceStart()
{
    API.onPlayerConnected += OnPlayerConnected;
}
```
### 2. Не подписывайся на одно и то же событие дважды
Иначе твой код будет выполняться два раза.

### 3. Проверяй значения перед использованием
Особенно это важно для:

- object[] args
- NetHandle
- string reason
- данных игрока и транспорта

## Быстрый список событий
### Событие	Описание
```
onResourceStart	Запуск текущего ресурса
onResourceStop	Остановка текущего ресурса
onUpdate	Серверный тик
onChatMessage	Сообщение игрока в чат
onChatCommand	Ввод команды в чат
onPlayerBeginConnect	Начало подключения игрока
onPlayerConnected	Игрок подключился
onPlayerFinishedDownload	Игрок закончил загрузку
onPlayerDisconnected	Игрок отключился
onPlayerDeath	Игрок умер
onPlayerRespawn	Игрок возродился
onClientEventTrigger	Клиент отправил событие на сервер
onPlayerPickup	Игрок поднял pickup
onPickupRespawn	Pickup появился заново
onMapChange	Сервер сменил карту
onPlayerEnterVehicle	Игрок сел в транспорт
onPlayerExitVehicle	Игрок вышел из транспорта
onVehicleDeath	Транспорт уничтожен
onEntityEnterColShape	Сущность вошла в ColShape
onEntityExitColShape	Сущность вышла из ColShape
onEntityDataChange	Изменились данные сущности
onServerResourceStart	Запустился любой ресурс на сервере
onServerResourceStop	Остановился любой ресурс на сервере
onVehicleHealthChange	Изменилось здоровье транспорта
onPlayerHealthChange	Изменилось здоровье игрока
onPlayerArmorChange	Изменилась броня игрока
onPlayerWeaponSwitch	Игрок сменил оружие
onPlayerWeaponAmmoChange	Изменились патроны
onVehicleSirenToggle	Включилась или выключилась сирена
onVehicleDoorBreak	У транспорта отломалась дверь
onVehicleWindowSmash	У транспорта разбилось стекло
onVehicleTyreBurst	У транспорта лопнуло колесо
onVehicleTrailerChange	Изменился прицеп
onPlayerModelChange	Игрок сменил модель
onPlayerDetonateStickies	Игрок подорвал sticky bombs
```

## События ресурса
### onResourceStart
Когда срабатывает: при запуске текущего ресурса.

#### Сигнатура:
```
API.onResourceStart += OnResourceStart;

private void OnResourceStart()
{
}
```
#### Пример:
```
private void OnResourceStart()
{
    API.consoleOutput("[MyResource] Ресурс запущен.");
}
```

### onResourceStop
Когда срабатывает: при остановке текущего ресурса.

#### Сигнатура:
```
API.onResourceStop += OnResourceStop;

private void OnResourceStop()
{
}
```
#### Пример:
```
csharp

private void OnResourceStop()
{
    API.consoleOutput("[MyResource] Ресурс остановлен.");
}
```
### После остановки ресурса его сущности обычно удаляются автоматически ядром.

### onUpdate
Когда срабатывает: очень часто, каждый тик сервера.

#### Сигнатура:
```
API.onUpdate += OnUpdate;

private void OnUpdate()
{
}
```
#### Пример:
```
using System;

private DateTime _lastUpdate = DateTime.Now;

private void OnUpdate()
{
    if ((DateTime.Now - _lastUpdate).TotalSeconds < 5) return;
    _lastUpdate = DateTime.Now;

    API.consoleOutput("Прошло 5 секунд.");
}
```

### ⚠️ Не делай тяжёлые операции внутри onUpdate на каждом тике.

# События чата и команд
## onChatMessage
Когда срабатывает: когда игрок пишет сообщение в чат.

#### Сигнатура:
```
API.onChatMessage += OnChatMessage;

private void OnChatMessage(Client player, string message, CancelEventArgs cancel)
{
}
```
#### Параметры:
```
Параметр	Тип	Описание
player	Client	Игрок
message	string	Текст сообщения
cancel	CancelEventArgs	Позволяет отменить показ сообщения
```
#### Пример:
```
private void OnChatMessage(Client player, string message, CancelEventArgs cancel)
{
    API.consoleOutput("[CHAT] " + player.Name + ": " + message);
}
```
Пример с блокировкой сообщения:
```
private void OnChatMessage(Client player, string message, CancelEventArgs cancel)
{
    if (message.ToLower().Contains("badword"))
    {
        cancel.Cancel = true;
        API.sendChatMessageToPlayer(player, "~r~Это слово запрещено.");
    }
}
```
## onChatCommand
Когда срабатывает: когда игрок вводит команду в чат.

#### Сигнатура:
```
API.onChatCommand += OnChatCommand;

private void OnChatCommand(Client player, string command, CancelEventArgs cancel)
{
}
```
#### Параметры:
```
Параметр	Тип	Описание
player	Client	Игрок
command	string	Введённая команда
cancel	CancelEventArgs	Можно запретить выполнение
```
#### Пример:
```
private void OnChatCommand(Client player, string command, CancelEventArgs cancel)
{
    API.consoleOutput("[CMD] " + player.Name + ": " + command);
}
```
#### Пример с отменой команды:
```
private void OnChatCommand(Client player, string command, CancelEventArgs cancel)
{
    if (command.StartsWith("/test"))
    {
        cancel.Cancel = true;
        API.sendChatMessageToPlayer(player, "~r~Команда /test отключена.");
    }
}
```
# События подключения игрока
## onPlayerBeginConnect
Когда срабатывает: когда игрок только начинает подключаться.

#### Сигнатура:
```
API.onPlayerBeginConnect += OnPlayerBeginConnect;

private void OnPlayerBeginConnect(Client player, CancelEventArgs cancelConnection)
{
}
```
#### Параметры:
```
Параметр	Тип	Описание
player	Client	Игрок
cancelConnection	CancelEventArgs	Можно запретить вход
```
#### Пример:
```
private void OnPlayerBeginConnect(Client player, CancelEventArgs cancelConnection)
{
    if (player.Name == "BadPlayer")
    {
        cancelConnection.Cancel = true;
        cancelConnection.Reason = "Вход запрещён.";
    }
}
```
## onPlayerConnected
Когда срабатывает: когда игрок успешно подключился к серверу.

#### Сигнатура:
```
API.onPlayerConnected += OnPlayerConnected;

private void OnPlayerConnected(Client player)
{
}
```
#### Пример:
```
private void OnPlayerConnected(Client player)
{
    API.sendChatMessageToAll("~g~► ~w~" + player.Name + " подключился!");
}
```
## onPlayerFinishedDownload
Когда срабатывает: когда игрок закончил загрузку файлов и готов к игре.

#### Сигнатура:
```
API.onPlayerFinishedDownload += OnPlayerFinishedDownload;

private void OnPlayerFinishedDownload(Client player)
{
}
```
#### Пример:
```
private void OnPlayerFinishedDownload(Client player)
{
    API.setEntityPosition(player.handle, new Vector3(-269.0f, -955.0f, 31.0f));
    API.setPlayerHealth(player, 100);
    API.sendChatMessageToPlayer(player, "~g~Добро пожаловать на сервер!");
}
```
## onPlayerDisconnected
Когда срабатывает: когда игрок отключился.

#### Сигнатура:
```
API.onPlayerDisconnected += OnPlayerDisconnected;

private void OnPlayerDisconnected(Client player, string reason)
{
}
```
#### Параметры:
```
Параметр	Тип	Описание
player	Client	Игрок
reason	string	Причина отключения
```
#### Пример:
```
private void OnPlayerDisconnected(Client player, string reason)
{
    API.sendChatMessageToAll("~r~◄ ~w~" + player.Name + " отключился. (" + reason + ")");
}
```
# События жизни игрока
## onPlayerDeath
Когда срабатывает: когда игрок умер.

#### Сигнатура:
```
API.onPlayerDeath += OnPlayerDeath;

private void OnPlayerDeath(Client player, NetHandle entityKiller, int weapon)
{
}
```
#### Параметры:

Параметр	Тип	Описание
player	Client	Погибший игрок
entityKiller	NetHandle	Кто убил
weapon	int	Оружие, которым убили
Пример:

csharp

private void OnPlayerDeath(Client player, NetHandle entityKiller, int weapon)
{
    Client killer = API.getPlayerFromHandle(entityKiller);

    if (killer != null && killer != player)
    {
        API.sendChatMessageToAll("~r~💀 ~w~" + player.Name + " убит игроком " + killer.Name);
    }
    else
    {
        API.sendChatMessageToAll("~r~💀 ~w~" + player.Name + " погиб.");
    }
}
onPlayerRespawn
Когда срабатывает: когда игрок возродился.

Сигнатура:

csharp

API.onPlayerRespawn += OnPlayerRespawn;

private void OnPlayerRespawn(Client player)
{
}
Пример:

csharp

private void OnPlayerRespawn(Client player)
{
    API.setPlayerArmor(player, 25);
    API.sendChatMessageToPlayer(player, "~g~Ты возродился.");
}
onPlayerHealthChange
Когда срабатывает: когда изменяется здоровье игрока.

Сигнатура:

csharp

API.onPlayerHealthChange += OnPlayerHealthChange;

private void OnPlayerHealthChange(Client player, int oldValue)
{
}
Пример:

csharp

private void OnPlayerHealthChange(Client player, int oldValue)
{
    int newHealth = API.getPlayerHealth(player);
    API.consoleOutput(player.Name + ": HP " + oldValue + " -> " + newHealth);
}
onPlayerArmorChange
Когда срабатывает: когда изменяется броня игрока.

Сигнатура:

csharp

API.onPlayerArmorChange += OnPlayerArmorChange;

private void OnPlayerArmorChange(Client player, int oldValue)
{
}
Пример:

csharp

private void OnPlayerArmorChange(Client player, int oldValue)
{
    int newArmor = API.getPlayerArmor(player);
    API.consoleOutput(player.Name + ": Armor " + oldValue + " -> " + newArmor);
}
onPlayerWeaponSwitch
Когда срабатывает: когда игрок меняет текущее оружие.

Сигнатура:

csharp

API.onPlayerWeaponSwitch += OnPlayerWeaponSwitch;

private void OnPlayerWeaponSwitch(Client player, WeaponHash oldValue)
{
}
Пример:

csharp

private void OnPlayerWeaponSwitch(Client player, WeaponHash oldValue)
{
    API.consoleOutput(player.Name + " сменил оружие с " + oldValue + " на " + player.CurrentWeapon);
}
onPlayerWeaponAmmoChange
Когда срабатывает: когда меняется количество патронов.

Сигнатура:

csharp

API.onPlayerWeaponAmmoChange += OnPlayerWeaponAmmoChange;

private void OnPlayerWeaponAmmoChange(Client player, WeaponHash weapon, int oldValue)
{
}
Пример:

csharp

private void OnPlayerWeaponAmmoChange(Client player, WeaponHash weapon, int oldValue)
{
    int newAmmo = API.getPlayerWeaponAmmo(player, weapon);
    API.consoleOutput(player.Name + ": " + weapon + " ammo " + oldValue + " -> " + newAmmo);
}
onPlayerModelChange
Когда срабатывает: когда игрок меняет модель персонажа.

Сигнатура:

csharp

API.onPlayerModelChange += OnPlayerModelChange;

private void OnPlayerModelChange(Client player, int oldValue)
{
}
Пример:

csharp

private void OnPlayerModelChange(Client player, int oldValue)
{
    API.consoleOutput(player.Name + " сменил модель. Старый hash: " + oldValue);
}
onPlayerDetonateStickies
Когда срабатывает: когда игрок подрывает sticky bombs.

Сигнатура:

csharp

API.onPlayerDetonateStickies += OnPlayerDetonateStickies;

private void OnPlayerDetonateStickies(Client player)
{
}
Пример:

csharp

private void OnPlayerDetonateStickies(Client player)
{
    API.consoleOutput(player.Name + " подорвал sticky bombs.");
}
События транспорта
onPlayerEnterVehicle
Когда срабатывает: когда игрок садится в транспорт.

Сигнатура:

csharp

API.onPlayerEnterVehicle += OnPlayerEnterVehicle;

private void OnPlayerEnterVehicle(Client player, NetHandle vehicle)
{
}
Пример:

csharp

private void OnPlayerEnterVehicle(Client player, NetHandle vehicle)
{
    int seat = API.getPlayerVehicleSeat(player);
    API.consoleOutput(player.Name + " сел в транспорт. Seat: " + seat);
}
Для водителя seat обычно равно -1.

onPlayerExitVehicle
Когда срабатывает: когда игрок выходит из транспорта.

Сигнатура:

csharp

API.onPlayerExitVehicle += OnPlayerExitVehicle;

private void OnPlayerExitVehicle(Client player, NetHandle vehicle)
{
}
Пример:

csharp

private void OnPlayerExitVehicle(Client player, NetHandle vehicle)
{
    API.consoleOutput(player.Name + " вышел из транспорта.");
}
onVehicleDeath
Когда срабатывает: когда транспорт уничтожен.

Сигнатура:

csharp

API.onVehicleDeath += OnVehicleDeath;

private void OnVehicleDeath(NetHandle vehicle)
{
}
Пример:

csharp

private void OnVehicleDeath(NetHandle vehicle)
{
    API.consoleOutput("Транспорт уничтожен.");
}
onVehicleHealthChange
Когда срабатывает: когда меняется здоровье транспорта.

Сигнатура:

csharp

API.onVehicleHealthChange += OnVehicleHealthChange;

private void OnVehicleHealthChange(NetHandle vehicle, float oldValue)
{
}
Пример:

csharp

private void OnVehicleHealthChange(NetHandle vehicle, float oldValue)
{
    float newHealth = API.getVehicleHealth(vehicle);
    API.consoleOutput("Vehicle health: " + oldValue + " -> " + newHealth);
}
onVehicleSirenToggle
Когда срабатывает: когда меняется состояние сирены.

Сигнатура:

csharp

API.onVehicleSirenToggle += OnVehicleSirenToggle;

private void OnVehicleSirenToggle(NetHandle vehicle, bool oldValue)
{
}
Пример:

csharp

private void OnVehicleSirenToggle(NetHandle vehicle, bool oldValue)
{
    bool current = API.getVehicleSirenState(vehicle);
    API.consoleOutput("Siren: " + oldValue + " -> " + current);
}
onVehicleDoorBreak
Когда срабатывает: когда у транспорта отламывается дверь.

Сигнатура:

csharp

API.onVehicleDoorBreak += OnVehicleDoorBreak;

private void OnVehicleDoorBreak(NetHandle vehicle, int index)
{
}
Пример:

csharp

private void OnVehicleDoorBreak(NetHandle vehicle, int index)
{
    API.consoleOutput("У транспорта отломалась дверь: " + index);
}
onVehicleWindowSmash
Когда срабатывает: когда у транспорта разбивается стекло.

Сигнатура:

csharp

API.onVehicleWindowSmash += OnVehicleWindowSmash;

private void OnVehicleWindowSmash(NetHandle vehicle, int index)
{
}
Пример:

csharp

private void OnVehicleWindowSmash(NetHandle vehicle, int index)
{
    API.consoleOutput("У транспорта разбилось стекло: " + index);
}
onVehicleTyreBurst
Когда срабатывает: когда у транспорта лопается колесо.

Сигнатура:

csharp

API.onVehicleTyreBurst += OnVehicleTyreBurst;

private void OnVehicleTyreBurst(NetHandle vehicle, int index)
{
}
Пример:

csharp

private void OnVehicleTyreBurst(NetHandle vehicle, int index)
{
    API.consoleOutput("У транспорта лопнуло колесо: " + index);
}
onVehicleTrailerChange
Когда срабатывает: когда меняется прицеп у транспорта.

Сигнатура:

csharp

API.onVehicleTrailerChange += OnVehicleTrailerChange;

private void OnVehicleTrailerChange(NetHandle tower, NetHandle trailer)
{
}
Пример:

csharp

private void OnVehicleTrailerChange(NetHandle tower, NetHandle trailer)
{
    if (trailer.IsNull)
        API.consoleOutput("Прицеп был отсоединён.");
    else
        API.consoleOutput("Прицеп был подключён.");
}
События pickup и карты
onPlayerPickup
Когда срабатывает: когда игрок поднимает pickup.

Сигнатура:

csharp

API.onPlayerPickup += OnPlayerPickup;

private void OnPlayerPickup(Client player, NetHandle pickupHandle)
{
}
Пример:

csharp

private void OnPlayerPickup(Client player, NetHandle pickupHandle)
{
    API.sendChatMessageToPlayer(player, "~g~Ты поднял pickup.");
}
onPickupRespawn
Когда срабатывает: когда pickup появляется заново.

Сигнатура:

csharp

API.onPickupRespawn += OnPickupRespawn;

private void OnPickupRespawn(NetHandle pickupHandle)
{
}
Пример:

csharp

private void OnPickupRespawn(NetHandle pickupHandle)
{
    API.consoleOutput("Pickup снова появился.");
}
onMapChange
Когда срабатывает: когда сервер меняет карту.

Сигнатура:

csharp

API.onMapChange += OnMapChange;

private void OnMapChange(string mapName, XmlGroup map)
{
}
Пример:

csharp

private void OnMapChange(string mapName, XmlGroup map)
{
    API.consoleOutput("Смена карты: " + mapName);
}
События ColShape
onEntityEnterColShape
Когда срабатывает: когда сущность входит в ColShape.

Сигнатура:

csharp

API.onEntityEnterColShape += OnEntityEnterColShape;

private void OnEntityEnterColShape(ColShape colshape, NetHandle entity)
{
}
Пример:

csharp

private ColShape _safeZone;

public MyResource()
{
    API.onResourceStart += OnResourceStart;
    API.onEntityEnterColShape += OnEntityEnterColShape;
}

private void OnResourceStart()
{
    _safeZone = API.createSphereColShape(new Vector3(0f, 0f, 72f), 10f);
}

private void OnEntityEnterColShape(ColShape colshape, NetHandle entity)
{
    if (colshape != _safeZone) return;

    Client player = API.getPlayerFromHandle(entity);
    if (player == null) return;

    API.sendChatMessageToPlayer(player, "~g~Ты вошёл в зону.");
}
onEntityExitColShape
Когда срабатывает: когда сущность выходит из ColShape.

Сигнатура:

csharp

API.onEntityExitColShape += OnEntityExitColShape;

private void OnEntityExitColShape(ColShape colshape, NetHandle entity)
{
}
Пример:

csharp

private void OnEntityExitColShape(ColShape colshape, NetHandle entity)
{
    Client player = API.getPlayerFromHandle(entity);
    if (player == null) return;

    API.sendChatMessageToPlayer(player, "~r~Ты покинул зону.");
}
События данных и ресурсов
onEntityDataChange
Когда срабатывает: когда у сущности изменяются данные и сервер вызывает это событие.

Сигнатура:

csharp

API.onEntityDataChange += OnEntityDataChange;

private void OnEntityDataChange(NetHandle entity, string key, object oldValue)
{
}
Пример:

csharp

private void OnEntityDataChange(NetHandle entity, string key, object oldValue)
{
    API.consoleOutput("Entity data changed: " + key);
}
onServerResourceStart
Когда срабатывает: когда на сервере запускается любой ресурс.

Сигнатура:

csharp

API.onServerResourceStart += OnServerResourceStart;

private void OnServerResourceStart(string resourceName)
{
}
Пример:

csharp

private void OnServerResourceStart(string resourceName)
{
    API.consoleOutput("Ресурс запущен: " + resourceName);
}
onServerResourceStop
Когда срабатывает: когда на сервере останавливается любой ресурс.

Сигнатура:

csharp

API.onServerResourceStop += OnServerResourceStop;

private void OnServerResourceStop(string resourceName)
{
}
Пример:

csharp

private void OnServerResourceStop(string resourceName)
{
    API.consoleOutput("Ресурс остановлен: " + resourceName);
}
События клиента
onClientEventTrigger
Когда срабатывает: когда клиент отправляет событие на сервер.

Сигнатура:

csharp

API.onClientEventTrigger += OnClientEventTrigger;

private void OnClientEventTrigger(Client player, string eventName, params object[] arguments)
{
}
Параметры:

Параметр	Тип	Описание
player	Client	Игрок
eventName	string	Имя события
arguments	object[]	Аргументы события
Пример:

csharp

private void OnClientEventTrigger(Client player, string eventName, params object[] arguments)
{
    API.consoleOutput("Client event: " + eventName);

    if (eventName == "playerBuyItem")
    {
        if (arguments == null || arguments.Length < 2) return;

        string itemName = arguments[0].ToString();
        int amount = Convert.ToInt32(arguments[1]);

        API.consoleOutput(player.Name + " wants to buy " + itemName + " x" + amount);
    }
}
⚠️ Всегда проверяй arguments.Length, прежде чем брать arguments[0], arguments[1] и так далее.

Полный шаблон ресурса с основными событиями
csharp

using System;
using GTANetworkServer;
using GTANetworkShared;

public class MyResource : Script
{
    public MyResource()
    {
        API.onResourceStart += OnResourceStart;
        API.onResourceStop += OnResourceStop;
        API.onPlayerBeginConnect += OnPlayerBeginConnect;
        API.onPlayerConnected += OnPlayerConnected;
        API.onPlayerFinishedDownload += OnPlayerFinishedDownload;
        API.onPlayerDisconnected += OnPlayerDisconnected;
        API.onPlayerDeath += OnPlayerDeath;
        API.onPlayerRespawn += OnPlayerRespawn;
        API.onChatMessage += OnChatMessage;
        API.onChatCommand += OnChatCommand;
        API.onPlayerEnterVehicle += OnPlayerEnterVehicle;
        API.onPlayerExitVehicle += OnPlayerExitVehicle;
        API.onClientEventTrigger += OnClientEventTrigger;
    }

    private void OnResourceStart()
    {
        API.consoleOutput("[MyResource] Started.");
    }

    private void OnResourceStop()
    {
        API.consoleOutput("[MyResource] Stopped.");
    }

    private void OnPlayerBeginConnect(Client player, CancelEventArgs cancelConnection)
    {
    }

    private void OnPlayerConnected(Client player)
    {
        API.sendChatMessageToAll("~g~► ~w~" + player.Name + " joined the server.");
    }

    private void OnPlayerFinishedDownload(Client player)
    {
        API.setEntityPosition(player.handle, new Vector3(-269.0f, -955.0f, 31.0f));
        API.setPlayerHealth(player, 100);
    }

    private void OnPlayerDisconnected(Client player, string reason)
    {
        API.sendChatMessageToAll("~r~◄ ~w~" + player.Name + " left the server.");
    }

    private void OnPlayerDeath(Client player, NetHandle entityKiller, int weapon)
    {
        API.sendChatMessageToAll("~r~" + player.Name + " died.");
    }

    private void OnPlayerRespawn(Client player)
    {
    }

    private void OnChatMessage(Client player, string message, CancelEventArgs cancel)
    {
        API.consoleOutput("[CHAT] " + player.Name + ": " + message);
    }

    private void OnChatCommand(Client player, string command, CancelEventArgs cancel)
    {
        API.consoleOutput("[CMD] " + player.Name + ": " + command);
    }

    private void OnPlayerEnterVehicle(Client player, NetHandle vehicle)
    {
        API.consoleOutput(player.Name + " entered vehicle.");
    }

    private void OnPlayerExitVehicle(Client player, NetHandle vehicle)
    {
        API.consoleOutput(player.Name + " exited vehicle.");
    }

    private void OnClientEventTrigger(Client player, string eventName, params object[] arguments)
    {
        API.consoleOutput("[EVENT] " + player.Name + ": " + eventName);
    }
}
Частые ошибки
Ошибка 1 — подписка не в конструкторе
Неправильно:

csharp

private void OnResourceStart()
{
    API.onPlayerConnected += OnPlayerConnected;
}
Правильно:

csharp

public MyResource()
{
    API.onPlayerConnected += OnPlayerConnected;
}
Ошибка 2 — нет проверки аргументов
Неправильно:

csharp

string item = arguments[0].ToString();
Правильно:

csharp

if (arguments == null || arguments.Length < 1) return;
string item = arguments[0].ToString();
Ошибка 3 — тяжёлый код в onUpdate
Неправильно:

csharp

private void OnUpdate()
{
    // тяжёлый код каждый тик
}
Правильно:

csharp

private DateTime _lastCheck = DateTime.Now;

private void OnUpdate()
{
    if ((DateTime.Now - _lastCheck).TotalSeconds < 1) return;
    _lastCheck = DateTime.Now;

    // код раз в секунду
}
Ошибка 4 — нет проверки убийцы
Неправильно:

csharp

API.sendChatMessageToAll("Killed by " + API.getPlayerFromHandle(entityKiller).Name);
Правильно:

csharp

Client killer = API.getPlayerFromHandle(entityKiller);
if (killer != null)
{
    API.sendChatMessageToAll("Killed by " + killer.Name);
}

