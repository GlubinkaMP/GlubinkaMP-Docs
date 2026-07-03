👤 Players

Документация по работе с игроками в **Glubinka MP SDK**.

Этот файл описывает именно удобный SDK-слой `GlubinkaMP.SDK`, а не низкоуровневые методы ядра.  
Если ты пишешь ресурс через `GlubinkaContext`, то обычно работаешь именно с классом `Player`.

---

## Что такое Player

`Player` — это публичная обёртка над внутренним объектом игрока.

Она нужна для того, чтобы разработчику ресурса не приходилось постоянно вызывать низкоуровневые методы API вручную.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как получить Player

В командах и событиях ядро часто передаёт `Client`.  
Чтобы превратить его в `Player`, используй метод `WrapPlayer(client)` из `GlubinkaContext`.

### Пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class PlayerExamples : GlubinkaContext
{
    [Command("whoami")]
    public void WhoAmI(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Твоё имя: ~w~" + player.Name);
    }
}
```

---

## Основные свойства

У объекта `Player` есть следующие свойства:

```csharp
string name = player.Name;
string socialClub = player.SocialClubName;
Vector3 position = player.Position;
Vector3 rotation = player.Rotation;
Vector3 velocity = player.Velocity;
int health = player.Health;
int armor = player.Armor;
int ping = player.Ping;
bool inVehicle = player.IsInVehicle;
int currentWeapon = player.CurrentWeapon;
int modelHash = player.ModelHash;
```

### Что они означают

| Свойство | Тип | Описание |
|---|---|---|
| `Name` | `string` | Ник игрока |
| `SocialClubName` | `string` | Имя Social Club |
| `Position` | `Vector3` | Текущая позиция |
| `Rotation` | `Vector3` | Текущий поворот |
| `Velocity` | `Vector3` | Текущая скорость |
| `Health` | `int` | Здоровье игрока |
| `Armor` | `int` | Броня игрока |
| `Ping` | `int` | Пинг игрока |
| `IsInVehicle` | `bool` | Находится ли игрок в транспорте |
| `CurrentWeapon` | `int` | Хэш текущего оружия |
| `ModelHash` | `int` | Хэш текущей модели персонажа |

---

## Здоровье

### Установить здоровье

```csharp
player.SetHealth(100);
```

### Убить игрока

```csharp
player.Kill();
```

### Пример команды

```csharp
[Command("healme")]
public void HealMe(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
    player.SendMessage("~g~Ты вылечен.");
}
```

> Допустимое значение здоровья в SDK: от `0` до `200`.

---

## Броня

### Установить броню

```csharp
player.SetArmor(100);
```

### Пример команды

```csharp
[Command("armor")]
public void ArmorCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SetArmor(100);
    player.SendMessage("~b~Броня выдана.");
}
```

> Допустимое значение брони: от `0` до `100`.

---

## Позиция и телепорт

### Телепортировать по `Vector3`

```csharp
player.Teleport(new Vector3(-269.0f, -955.0f, 31.0f));
```

### Телепортировать по координатам

```csharp
player.Teleport(-269.0f, -955.0f, 31.0f);
```

### Пример команды

```csharp
[Command("gotoairport")]
public void GoToAirport(Client client)
{
    var player = WrapPlayer(client);
    player.Teleport(-1034.0f, -2733.0f, 20.0f);
    player.SendMessage("~y~Ты телепортирован в аэропорт.");
}
```

---

## Скин персонажа

### Установить скин по `PedHash`

```csharp
player.SetSkin(PedHash.Cop01SFY);
```

### Установить скин по числовому хэшу

```csharp
player.SetSkin((int)PedHash.Fireman01SMY);
```

### Популярные модели

| PedHash | Описание |
|---|---|
| `PedHash.Michael` | Майкл |
| `PedHash.Franklin` | Франклин |
| `PedHash.Trevor` | Тревор |
| `PedHash.Cop01SFY` | Полицейский |
| `PedHash.Doctor01SMM` | Доктор |
| `PedHash.Fireman01SMY` | Пожарный |
| `PedHash.Paramedic01SMM` | Парамедик |
| `PedHash.FreemodeMale01` | Фримод мужчина |
| `PedHash.FreemodeFemale01` | Фримод женщина |

### Пример команды

```csharp
[Command("copskin")]
public void CopSkin(Client client)
{
    var player = WrapPlayer(client);
    player.SetSkin(PedHash.Cop01SFY);
    player.SendMessage("~b~Твой скин изменён.");
}
```

---

## Оружие

### Выдать оружие

```csharp
player.GiveWeapon(WeaponHash.Pistol, 100);
```

### Забрать одно оружие

```csharp
player.RemoveWeapon(WeaponHash.Pistol);
```

### Забрать всё оружие

```csharp
player.RemoveAllWeapons();
```

### Популярные оружия

| WeaponHash | Описание |
|---|---|
| `WeaponHash.Pistol` | Пистолет |
| `WeaponHash.CombatPistol` | Боевой пистолет |
| `WeaponHash.SMG` | SMG |
| `WeaponHash.AssaultRifle` | Штурмовая винтовка |
| `WeaponHash.CarbineRifle` | Карабин |
| `WeaponHash.SniperRifle` | Снайперская винтовка |
| `WeaponHash.Shotgun` | Дробовик |
| `WeaponHash.Knife` | Нож |
| `WeaponHash.Bat` | Бита |

### Пример команды

```csharp
[Command("pistol")]
public void PistolCommand(Client client)
{
    var player = WrapPlayer(client);
    player.GiveWeapon(WeaponHash.Pistol, 60);
    player.SendMessage("~g~Ты получил пистолет.");
}
```

---

## Сообщения игроку

### Простое сообщение

```csharp
player.SendMessage("Привет!");
```

### Цветное сообщение

```csharp
player.SendMessage("~g~", "Добро пожаловать!");
```

### Примеры цветов

| Код | Цвет |
|---|---|
| `~r~` | Красный |
| `~g~` | Зелёный |
| `~b~` | Синий |
| `~y~` | Жёлтый |
| `~w~` | Белый |
| `~p~` | Фиолетовый |
| `~o~` | Оранжевый |

---

## Работа с транспортом

`Player` умеет взаимодействовать с объектом `Vehicle` из SDK.

### Посадить игрока в транспорт

```csharp
player.PutIntoVehicle(vehicle);
```

По умолчанию игрок садится на место водителя.

### Посадить игрока на конкретное место

```csharp
player.PutIntoVehicle(vehicle, 0);
```

### Значения сидений

| Seat | Описание |
|---|---|
| `-1` | Водитель |
| `0` | Передний пассажир |
| `1` | Заднее место |
| `2` | Следующее заднее место |

### Высадить игрока из транспорта

```csharp
player.RemoveFromVehicle();
```

> В текущей версии старого ядра высадка работает через серверное принудительное выведение игрока из машины. В редких случаях положение игрока после высадки может зависеть от положения транспорта.

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class PlayerDemo : GlubinkaContext
{
    [Command("starterkit")]
    public void StarterKit(Client client)
    {
        var player = WrapPlayer(client);

        player.SetHealth(100);
        player.SetArmor(100);
        player.SetSkin(PedHash.Cop01SFY);
        player.GiveWeapon(WeaponHash.Pistol, 90);
        player.SendMessage("~g~Ты получил стартовый набор.");
    }

    [Command("killme")]
    public void KillMe(Client client)
    {
        var player = WrapPlayer(client);
        player.Kill();
    }

    [Command("spawncity")]
    public void SpawnCity(Client client)
    {
        var player = WrapPlayer(client);
        player.Teleport(-269.0f, -955.0f, 31.0f);
        player.SendMessage("~y~Ты перемещён в город.");
    }
}
```

---

## Как получить Player для другого игрока

Если тебе нужен игрок по имени, можно использовать низкоуровневый API, а потом обернуть его в `Player`.

```csharp
Client targetClient = API.getPlayerFromName("Ivan");

if (targetClient == null)
{
    return;
}

var target = WrapPlayer(targetClient);
target.SendMessage("~g~Тебя нашли.");
```

---

## Частые ошибки

### Ошибка 1 — забыл обернуть `Client` в `Player`

Неправильно:

```csharp
public void HealMe(Client client)
{
    client.SetHealth(100);
}
```

Правильно:

```csharp
public void HealMe(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
}
```

---

### Ошибка 2 — передал неправильное значение здоровья

Неправильно:

```csharp
player.SetHealth(999);
```

Правильно:

```csharp
player.SetHealth(100);
```

---

### Ошибка 3 — работаешь с `Vehicle`, но передал `null`

Неправильно:

```csharp
player.PutIntoVehicle(null);
```

Правильно:

```csharp
var vehicle = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
player.PutIntoVehicle(vehicle);
```

---

### Ошибка 4 — думаешь, что `CurrentWeapon` уже `WeaponHash`

Свойство `CurrentWeapon` в SDK сейчас возвращает `int`.

Если нужен enum:

```csharp
WeaponHash weapon = (WeaponHash)player.CurrentWeapon;
```

---

## См. также

- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
