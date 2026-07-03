# 🚗 Vehicles

Документация по работе с транспортом в **Glubinka MP SDK**.

Этот файл описывает именно класс `Vehicle` из `GlubinkaMP.SDK` — удобную обёртку для создания и управления транспортом в ресурсах.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Что такое Vehicle

`Vehicle` — это SDK-обёртка над транспортом в игровом мире.

Обычно объект `Vehicle` создаётся через метод `CreateVehicle(...)` внутри класса `GlubinkaContext`.

---

## Как создать транспорт

### Базовый пример

```csharp
var vehicle = CreateVehicle(
    VehicleHash.Adder,
    new Vector3(0f, 0f, 72f),
    new Vector3(0f, 0f, 0f)
);
```

### Создание рядом с игроком

```csharp
[Command("car")]
public void CarCommand(Client client)
{
    var player = WrapPlayer(client);

    var spawnPos = new Vector3(
        player.Position.X + 3f,
        player.Position.Y,
        player.Position.Z
    );

    var vehicle = CreateVehicle(
        VehicleHash.Adder,
        spawnPos,
        new Vector3(0f, 0f, 0f)
    );

    player.SendMessage("~g~Транспорт создан.");
}
```

### Создание и посадка игрока

```csharp
[Command("mycar")]
public void MyCarCommand(Client client)
{
    var player = WrapPlayer(client);

    var spawnPos = new Vector3(
        player.Position.X + 3f,
        player.Position.Y,
        player.Position.Z
    );

    var vehicle = CreateVehicle(
        VehicleHash.Adder,
        spawnPos,
        new Vector3(0f, 0f, 0f)
    );

    vehicle.SetColor(111, 111);
    player.PutIntoVehicle(vehicle);
    player.SendMessage("~g~Твоя машина готова.");
}
```

---

## Основные свойства

У объекта `Vehicle` есть следующие свойства:

```csharp
NetHandle handle = vehicle.Handle;
Vector3 position = vehicle.Position;
Vector3 rotation = vehicle.Rotation;
bool alive = vehicle.IsAlive;
float health = vehicle.Health;
```

### Что они означают

| Свойство | Тип | Описание |
|---|---|---|
| `Handle` | `NetHandle` | Внутренний идентификатор транспорта |
| `Position` | `Vector3` | Позиция транспорта |
| `Rotation` | `Vector3` | Поворот транспорта |
| `IsAlive` | `bool` | Не уничтожен ли транспорт |
| `Health` | `float` | Здоровье транспорта |

---

## Позиция и поворот

### Получить позицию

```csharp
Vector3 pos = vehicle.Position;
```

### Установить позицию

```csharp
vehicle.Position = new Vector3(100f, 200f, 30f);
```

### Получить поворот

```csharp
Vector3 rot = vehicle.Rotation;
```

### Установить поворот

```csharp
vehicle.Rotation = new Vector3(0f, 0f, 90f);
```

---

## Здоровье транспорта

### Получить здоровье

```csharp
float hp = vehicle.Health;
```

### Установить здоровье

```csharp
vehicle.Health = 1000f;
```

### Проверить, жив ли транспорт

```csharp
bool alive = vehicle.IsAlive;
```

### Починить транспорт

```csharp
vehicle.Repair();
```

> Обычно `1000` — полностью исправный транспорт.  
> Если здоровье падает до `0`, транспорт считается уничтоженным.

---

## Удаление транспорта

### Удалить транспорт

```csharp
vehicle.Delete();
```

> После удаления использовать этот объект дальше уже не нужно.

---

## Цвета транспорта

### Установить сразу два цвета

```csharp
vehicle.SetColor(12, 111);
```

### Установить только основной цвет

```csharp
vehicle.SetPrimaryColor(12);
```

### Установить только дополнительный цвет

```csharp
vehicle.SetSecondaryColor(111);
```

### Примеры ID цветов

| ID | Цвет |
|---|---|
| `0` | Чёрный |
| `1` | Графит |
| `12` | Красный |
| `27` | Жёлтый |
| `38` | Синий |
| `51` | Зелёный |
| `111` | Оранжевый |
| `134` | Белый |

---

## Замки дверей

### Заблокировать двери

```csharp
vehicle.LockDoors();
```

### Разблокировать двери

```csharp
vehicle.UnlockDoors();
```

---

## Посадка игрока в транспорт

Объект `Vehicle` может сам посадить игрока в себя.

### Посадить водителем

```csharp
vehicle.PutPlayerInto(player);
```

### Посадить на конкретное место

```csharp
vehicle.PutPlayerInto(player, 0);
```

### Значения сидений

| Seat | Описание |
|---|---|
| `-1` | Водитель |
| `0` | Передний пассажир |
| `1` | Заднее место |
| `2` | Следующее заднее место |

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class VehicleDemo : GlubinkaContext
{
    [Command("spawncar")]
    public void SpawnCar(Client client)
    {
        var player = WrapPlayer(client);

        var spawnPos = new Vector3(
            player.Position.X + 3f,
            player.Position.Y,
            player.Position.Z
        );

        var vehicle = CreateVehicle(
            VehicleHash.Adder,
            spawnPos,
            new Vector3(0f, 0f, 0f)
        );

        vehicle.SetColor(12, 12);
        player.SendMessage("~g~Машина создана.");
    }

    [Command("mycar")]
    public void MyCar(Client client)
    {
        var player = WrapPlayer(client);

        var spawnPos = new Vector3(
            player.Position.X + 3f,
            player.Position.Y,
            player.Position.Z
        );

        var vehicle = CreateVehicle(
            VehicleHash.Adder,
            spawnPos,
            new Vector3(0f, 0f, 0f)
        );

        vehicle.SetColor(27, 27);
        vehicle.PutPlayerInto(player, -1);
        player.SendMessage("~g~Ты посажен в транспорт.");
    }

    [Command("carfix")]
    public void CarFix(Client client)
    {
        var player = WrapPlayer(client);

        var spawnPos = new Vector3(
            player.Position.X + 3f,
            player.Position.Y,
            player.Position.Z
        );

        var vehicle = CreateVehicle(
            VehicleHash.Adder,
            spawnPos,
            new Vector3(0f, 0f, 0f)
        );

        vehicle.Health = 200f;
        vehicle.Repair();
        player.SendMessage("~g~Транспорт починен.");
    }
}
```

---

## Популярные модели транспорта

### Спорткары

| VehicleHash | Название |
|---|---|
| `VehicleHash.Adder` | Adder |
| `VehicleHash.Zentorno` | Zentorno |
| `VehicleHash.T20` | T20 |
| `VehicleHash.Turismor` | Turismo R |
| `VehicleHash.Entityxf` | Entity XF |

### Мотоциклы

| VehicleHash | Название |
|---|---|
| `VehicleHash.Bati` | Bati 801 |
| `VehicleHash.Akuma` | Akuma |
| `VehicleHash.Hakuchou` | Hakuchou |
| `VehicleHash.Sanchez` | Sanchez |

### Спецтранспорт

| VehicleHash | Название |
|---|---|
| `VehicleHash.Police` | Полицейская машина |
| `VehicleHash.Ambulance` | Скорая помощь |
| `VehicleHash.FireTruck` | Пожарная машина |
| `VehicleHash.Taxi` | Такси |
| `VehicleHash.Bus` | Автобус |

### Воздушный транспорт

| VehicleHash | Название |
|---|---|
| `VehicleHash.Buzzard2` | Buzzard |
| `VehicleHash.Maverick` | Maverick |
| `VehicleHash.Lazer` | Lazer |
| `VehicleHash.Hydra` | Hydra |

---

## Если нужен низкоуровневый API

`Vehicle.Handle` можно передать в обычные методы ядра, если тебе нужна более тонкая настройка.

Пример:

```csharp
API.setVehicleNumberPlate(vehicle.Handle, "GLUBINKA");
```

Это полезно, если в SDK-обёртке пока ещё нет нужного метода.

---

## Частые ошибки

### Ошибка 1 — создаёшь транспорт прямо в игроке

Неправильно:

```csharp
var vehicle = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
```

Правильно:

```csharp
var spawnPos = new Vector3(
    player.Position.X + 3f,
    player.Position.Y,
    player.Position.Z
);

var vehicle = CreateVehicle(VehicleHash.Adder, spawnPos, new Vector3(0, 0, 0));
```

---

### Ошибка 2 — используешь объект после удаления

Неправильно:

```csharp
vehicle.Delete();
vehicle.Repair();
```

Правильно:

```csharp
vehicle.Delete();
```

---

### Ошибка 3 — передал `null` вместо игрока

Неправильно:

```csharp
vehicle.PutPlayerInto(null);
```

Правильно:

```csharp
vehicle.PutPlayerInto(player);
```

---

### Ошибка 4 — думаешь, что `Vehicle` создаётся в любой точке без проверки

Если транспорт создать внутри стены, в воздухе или слишком близко к другому объекту, в игре он может вести себя странно.

Всегда старайся выбирать безопасную позицию спавна.

---

## См. также

- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
