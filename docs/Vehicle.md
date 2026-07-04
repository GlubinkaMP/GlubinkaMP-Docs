---
title: Vehicle
nav_order: 4
---

# Vehicle

Документация по работе с транспортом в **Glubinka MP SDK**.

Этот файл описывает публичный SDK-класс `Vehicle` из пространства имён `GlubinkaMP.SDK`.

---

## Что такое Vehicle

`Vehicle` — это публичная обёртка над внутренним объектом транспорта.

Она даёт удобный доступ к управлению машинами, мотоциклами, лодками и любым другим транспортом на сервере — без необходимости работать с низкоуровневыми методами ядра напрямую.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как создать Vehicle

Транспорт создаётся через метод `CreateVehicle` из `GlubinkaContext`.

```csharp
var car = CreateVehicle(VehicleHash.Adder, new Vector3(0, 0, 72), new Vector3(0, 0, 0));
```

Возвращает объект `Vehicle` со всеми методами SDK.

### Пример

```csharp
[Command("spawncar")]
public void SpawnCar(Client client)
{
    var player = WrapPlayer(client);
    var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
    player.PutIntoVehicle(car);
    player.SendSuccess("Машина создана.");
}
```

---

## Как получить Vehicle из события

В некоторых событиях ядро передаёт `NetHandle` транспорта.
Чтобы превратить его в `Vehicle`, используй `WrapVehicle` из `GlubinkaContext`.

```csharp
var vehicle = WrapVehicle(handle);
```

---

## Основные свойства

```csharp
NetHandle handle  = vehicle.Handle;
Vector3 position  = vehicle.Position;
Vector3 rotation  = vehicle.Rotation;
bool alive        = vehicle.IsAlive;
float health      = vehicle.Health;
bool locked       = vehicle.Locked;
bool engineOn     = vehicle.EngineStatus;
string plate      = vehicle.NumberPlate;
bool siren        = vehicle.SirenState;
bool bulletproof  = vehicle.BulletproofTyres;
int livery        = vehicle.Livery;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Handle` | `NetHandle` | Внутренний handle транспорта |
| `Position` | `Vector3` | Позиция в мире (чтение / запись) |
| `Rotation` | `Vector3` | Поворот (чтение / запись) |
| `IsAlive` | `bool` | Не уничтожен ли транспорт |
| `Health` | `float` | Здоровье (чтение / запись) |
| `Locked` | `bool` | Заблокирован ли (чтение / запись) |
| `EngineStatus` | `bool` | Запущен ли двигатель (чтение / запись) |
| `NumberPlate` | `string` | Номерной знак (чтение / запись) |
| `SirenState` | `bool` | Включена ли сирена |
| `BulletproofTyres` | `bool` | Пуленепробиваемые шины (чтение / запись) |
| `Livery` | `int` | Ливрея (чтение / запись) |

---

## Цвета и внешний вид

```csharp
int pearlescentColor  = vehicle.PearlescentColor;
int wheelColor        = vehicle.WheelColor;
int wheelType         = vehicle.WheelType;
int windowTint        = vehicle.WindowTint;
int dashboardColor    = vehicle.DashboardColor;
int trimColor         = vehicle.TrimColor;
Color neonColor       = vehicle.NeonColor;
Color tyreSmokeColor  = vehicle.TyreSmokeColor;
int plateStyle        = vehicle.NumberPlateStyle;
```

| Свойство | Тип | Описание |
|---|---|---|
| `PearlescentColor` | `int` | Перламутровый цвет (чтение / запись) |
| `WheelColor` | `int` | Цвет дисков (чтение / запись) |
| `WheelType` | `int` | Тип колёс (чтение / запись) |
| `WindowTint` | `int` | Тонировка окон (чтение / запись) |
| `DashboardColor` | `int` | Цвет приборной панели (чтение / запись) |
| `TrimColor` | `int` | Цвет салона (чтение / запись) |
| `NeonColor` | `Color` | Цвет неона (чтение / запись) |
| `TyreSmokeColor` | `Color` | Цвет дыма из-под шин (чтение / запись) |
| `NumberPlateStyle` | `int` | Стиль номерного знака (чтение / запись) |

---

## Мощность двигателя

```csharp
float power  = vehicle.EnginePowerMultiplier;
float torque = vehicle.EngineTorqueMultiplier;

vehicle.EnginePowerMultiplier  = 2.0f;   // удвоить мощность
vehicle.EngineTorqueMultiplier = 2.0f;   // удвоить крутящий момент
```

| Свойство | Тип | Описание |
|---|---|---|
| `EnginePowerMultiplier` | `float` | Множитель мощности двигателя |
| `EngineTorqueMultiplier` | `float` | Множитель крутящего момента |

> Значение `1.0` — стандартное. `2.0` — вдвое мощнее.

---

## Пассажиры и водитель

```csharp
Player driver     = vehicle.Driver;
Player[] occupants = vehicle.Occupants;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Driver` | `Player` | Водитель транспорта (или `null` если нет) |
| `Occupants` | `Player[]` | Все игроки в транспорте |

### Try API — безопасное получение водителя

```csharp
if (vehicle.TryGetDriver(out Player driver))
{
    driver.SendMessage("~g~Ты водишь машину.");
}
```

### Прицепы

```csharp
NetHandle trailer    = vehicle.Trailer;
NetHandle trailedBy  = vehicle.TraileredBy;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Trailer` | `NetHandle` | Прицеп, который тянет этот транспорт |
| `TraileredBy` | `NetHandle` | Транспорт, который тянет этот |

---

## Основные действия

### Удалить транспорт

```csharp
vehicle.Delete();
```

### Починить транспорт

```csharp
vehicle.Repair();
```

### Взорвать транспорт

```csharp
vehicle.Explode();
```

### Установить здоровье

```csharp
vehicle.SetHealth(1000f);
vehicle.Health = 1000f;   // то же самое через свойство
```

### Переместить транспорт

```csharp
vehicle.MoveTo(new Vector3(100, 200, 30));
vehicle.MoveTo(100f, 200f, 30f);
```

### Повернуть транспорт

```csharp
vehicle.RotateTo(new Vector3(0, 0, 90));
vehicle.RotateTo(0f, 0f, 90f);
```

### Пример

```csharp
[Command("repaircar")]
public void RepairCar(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.Repair();
    player.SendSuccess("Машина починена.");
}
```

---

## Двигатель

```csharp
vehicle.StartEngine();          // запустить
vehicle.StopEngine();           // заглушить
vehicle.EngineStatus = true;    // запустить через свойство
vehicle.EngineStatus = false;   // заглушить через свойство
```

| Метод | Описание |
|---|---|
| `StartEngine()` | Запустить двигатель |
| `StopEngine()` | Заглушить двигатель |

### Пример

```csharp
[Command("engine")]
public void Engine(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);

    if (vehicle.EngineStatus)
    {
        vehicle.StopEngine();
        player.SendMessage("~r~Двигатель заглушён.");
    }
    else
    {
        vehicle.StartEngine();
        player.SendMessage("~g~Двигатель запущен.");
    }
}
```

---

## Замки

```csharp
vehicle.Lock();            // заблокировать
vehicle.Unlock();          // разблокировать
vehicle.LockDoors();       // alias для Lock()
vehicle.UnlockDoors();     // alias для Unlock()
vehicle.SetLocked(true);   // заблокировать через метод
vehicle.SetLocked(false);  // разблокировать через метод
vehicle.Locked = true;     // через свойство
```

| Метод | Описание |
|---|---|
| `Lock()` | Заблокировать транспорт |
| `Unlock()` | Разблокировать транспорт |
| `LockDoors()` | Alias для Lock() |
| `UnlockDoors()` | Alias для Unlock() |
| `SetLocked(bool)` | Заблокировать или разблокировать |

### Пример

```csharp
[Command("lock")]
public void LockCar(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.Lock();
    player.SendMessage("~b~Машина заблокирована.");
}
```

---

## Двери

Двери управляются через enum `VehicleDoor` или числовой ID.

### Открыть / закрыть дверь

```csharp
vehicle.OpenDoor(VehicleDoor.FrontLeft);
vehicle.CloseDoor(VehicleDoor.FrontLeft);
vehicle.SetDoorState(VehicleDoor.Hood, true);   // открыть капот
vehicle.SetDoorState(VehicleDoor.Trunk, false); // закрыть багажник
```

### Проверить состояние двери

```csharp
bool isOpen   = vehicle.GetDoorState(VehicleDoor.FrontLeft);
bool isBroken = vehicle.IsDoorBroken(VehicleDoor.FrontLeft);
```

### Enum `VehicleDoor`

| Значение | Число | Описание |
|---|---|---|
| `VehicleDoor.FrontLeft` | 0 | Передняя левая (водительская) |
| `VehicleDoor.FrontRight` | 1 | Передняя правая (пассажирская) |
| `VehicleDoor.RearLeft` | 2 | Задняя левая |
| `VehicleDoor.RearRight` | 3 | Задняя правая |
| `VehicleDoor.Trunk` | 4 | Багажник |
| `VehicleDoor.Hood` | 5 | Капот |

### Пример

```csharp
[Command("opentrunk")]
public void OpenTrunk(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.OpenDoor(VehicleDoor.Trunk);
    player.SendMessage("~g~Багажник открыт.");
}
```

---

## Колёса

### Проверить и пробить колесо

```csharp
bool popped = vehicle.IsTyrePopped(VehicleTyre.FrontLeft);
vehicle.PopTyre(VehicleTyre.FrontLeft);
```

### Enum `VehicleTyre`

| Значение | Число | Описание |
|---|---|---|
| `VehicleTyre.FrontLeft` | 0 | Переднее левое |
| `VehicleTyre.FrontRight` | 1 | Переднее правое |
| `VehicleTyre.MiddleLeft` | 2 | Среднее левое (грузовики) |
| `VehicleTyre.MiddleRight` | 3 | Среднее правое (грузовики) |
| `VehicleTyre.RearLeft` | 4 | Заднее левое |
| `VehicleTyre.RearRight` | 5 | Заднее правое |

### Пример

```csharp
[Command("poptyre")]
public void PopTyre(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.PopTyre(VehicleTyre.FrontLeft);
    player.SendMessage("~r~Переднее левое колесо пробито.");
}
```

---

## Окна

### Проверить состояние окна

```csharp
bool broken = vehicle.IsWindowBroken(VehicleWindow.FrontLeft);
```

### Enum `VehicleWindow`

| Значение | Число | Описание |
|---|---|---|
| `VehicleWindow.FrontLeft` | 0 | Переднее левое (водительское) |
| `VehicleWindow.FrontRight` | 1 | Переднее правое (пассажирское) |
| `VehicleWindow.RearLeft` | 2 | Заднее левое |
| `VehicleWindow.RearRight` | 3 | Заднее правое |

---

## Неон

### Включить / выключить конкретную сторону

```csharp
vehicle.EnableNeon(VehicleNeon.Left);
vehicle.DisableNeon(VehicleNeon.Left);
vehicle.SetNeonState(VehicleNeon.Front, true);
```

### Включить / выключить весь неон сразу

```csharp
vehicle.EnableAllNeon();
vehicle.DisableAllNeon();
vehicle.SetAllNeon(true);
vehicle.SetAllNeon(false);
```

### Проверить состояние неона

```csharp
bool active = vehicle.GetNeonState(VehicleNeon.Left);
```

### Установить цвет неона

```csharp
vehicle.SetNeonColor(255, 0, 255);         // через RGB
vehicle.NeonColor = new Color(255, 0, 255); // через свойство
```

### Enum `VehicleNeon`

| Значение | Число | Описание |
|---|---|---|
| `VehicleNeon.Left` | 0 | Левый неон |
| `VehicleNeon.Right` | 1 | Правый неон |
| `VehicleNeon.Front` | 2 | Передний неон |
| `VehicleNeon.Back` | 3 | Задний неон |

### Пример

```csharp
[Command("neon")]
public void Neon(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.EnableAllNeon();
    vehicle.SetNeonColor(0, 255, 255);
    player.SendSuccess("Неон включён.");
}
```

---

## Покраска

### Стандартные цвета (по индексу)

```csharp
vehicle.SetColor(27, 27);           // оба цвета сразу
vehicle.SetPrimaryColor(27);        // только основной
vehicle.SetSecondaryColor(0);       // только вторичный
```

### Кастомные RGB-цвета

```csharp
vehicle.SetCustomPrimaryColor(new Color(255, 0, 0));
vehicle.SetCustomPrimaryColor(255, 0, 0);

vehicle.SetCustomSecondaryColor(new Color(0, 0, 255));
vehicle.SetCustomSecondaryColor(0, 0, 255);
```

### Дополнительные цвета

```csharp
vehicle.PearlescentColor  = 10;
vehicle.WheelColor        = 5;
vehicle.DashboardColor    = 3;
vehicle.TrimColor         = 2;
vehicle.WindowTint        = 1;
```

### Цвет дыма из-под шин

```csharp
vehicle.SetTyreSmokeColor(255, 0, 0);
vehicle.TyreSmokeColor = new Color(255, 0, 0);
```

### Пример

```csharp
[Command("paintcar")]
public void PaintCar(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.SetCustomPrimaryColor(255, 0, 0);
    vehicle.SetCustomSecondaryColor(0, 0, 0);
    vehicle.PearlescentColor = 10;
    player.SendSuccess("Машина перекрашена.");
}
```

---

## Extras

Extras — это дополнительные детали модели транспорта (например, антенна, решётка, спойлер).

```csharp
bool hasExtra = vehicle.HasExtra(1);
vehicle.SetExtra(1, true);    // включить
vehicle.SetExtra(1, false);   // выключить
vehicle.EnableExtra(1);       // alias для SetExtra(1, true)
vehicle.DisableExtra(1);      // alias для SetExtra(1, false)
```

| Метод | Описание |
|---|---|
| `HasExtra(int)` | Проверить состояние extra |
| `SetExtra(int, bool)` | Включить или выключить extra |
| `EnableExtra(int)` | Включить extra |
| `DisableExtra(int)` | Выключить extra |

### Пример

```csharp
[Command("extra")]
public void Extra(Client client, int id)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.EnableExtra(id);
    player.SendSuccess("Extra " + id + " включён.");
}
```

---

## Моды

Моды — это тюнинг транспорта (двигатель, тормоза, подвеска и т.п.).

```csharp
int mod = vehicle.GetMod(11);    // получить текущий мод двигателя
vehicle.SetMod(11, 3);           // установить мод
vehicle.RemoveMod(11);           // убрать мод
```

| Метод | Описание |
|---|---|
| `GetMod(int modType)` | Получить установленный мод |
| `SetMod(int modType, int modIndex)` | Установить мод |
| `RemoveMod(int modType)` | Убрать мод |

### Популярные типы модов

| modType | Описание |
|---|---|
| `0` | Спойлер |
| `1` | Передний бампер |
| `2` | Задний бампер |
| `3` | Боковая юбка |
| `11` | Двигатель |
| `12` | Тормоза |
| `13` | Трансмиссия |
| `15` | Подвеска |
| `18` | Турбо |

### Пример

```csharp
[Command("tunecar")]
public void TuneCar(Client client)
{
    var player = WrapPlayer(client);

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.SetMod(11, 3);   // двигатель уровень 3
    vehicle.SetMod(12, 2);   // тормоза уровень 2
    vehicle.SetMod(13, 2);   // трансмиссия уровень 2
    vehicle.SetMod(15, 3);   // подвеска уровень 3
    player.SendSuccess("Машина прокачана.");
}
```

---

## Посадка игрока

### Через enum VehicleSeat

```csharp
vehicle.PutPlayerInto(player, VehicleSeat.Driver);
vehicle.PutPlayerInto(player, VehicleSeat.FrontPassenger);
```

### Через числовой ID

```csharp
vehicle.PutPlayerInto(player);       // водитель
vehicle.PutPlayerInto(player, 0);    // передний пассажир
```

### Enum `VehicleSeat`

| Значение | Число | Описание |
|---|---|---|
| `VehicleSeat.Any` | `-2` | Любое свободное место |
| `VehicleSeat.Driver` | `-1` | Водитель |
| `VehicleSeat.FrontPassenger` | `0` | Передний пассажир |
| `VehicleSeat.RearLeft` | `1` | Заднее левое |
| `VehicleSeat.RearRight` | `2` | Заднее правое |
| `VehicleSeat.Extra1` | `3` | Дополнительное место 1 |
| `VehicleSeat.Extra2` | `4` | Дополнительное место 2 |

### Пример

```csharp
[Command("getincar")]
public void GetInCar(Client client, string targetName)
{
    var player = WrapPlayer(client);

    if (!Players.TryGetByName(targetName, out Player target))
    {
        player.SendError("Игрок не найден.");
        return;
    }

    if (!player.IsInVehicle)
    {
        player.SendError("Ты не в машине.");
        return;
    }

    var vehicle = WrapVehicle(player.CurrentVehicle);
    vehicle.PutPlayerInto(target, VehicleSeat.FrontPassenger);
    player.SendSuccess("Игрок посажен в машину.");
}
```

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
        var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));

        car.SetCustomPrimaryColor(255, 0, 0);
        car.SetCustomSecondaryColor(0, 0, 0);
        car.NumberPlate = "GLUBINKA";
        car.StartEngine();

        player.PutIntoVehicle(car, VehicleSeat.Driver);
        player.SendSuccess("Машина создана и готова к езде.");
    }

    [Command("tunecar")]
    public void TuneCar(Client client)
    {
        var player = WrapPlayer(client);

        if (!player.IsInVehicle)
        {
            player.SendError("Ты не в машине.");
            return;
        }

        var vehicle = WrapVehicle(player.CurrentVehicle);
        vehicle.SetMod(11, 3);
        vehicle.SetMod(12, 2);
        vehicle.SetMod(13, 2);
        vehicle.SetMod(15, 3);
        vehicle.EnginePowerMultiplier = 2.0f;
        player.SendSuccess("Машина прокачана.");
    }

    [Command("neoncar")]
    public void NeonCar(Client client)
    {
        var player = WrapPlayer(client);

        if (!player.IsInVehicle)
        {
            player.SendError("Ты не в машине.");
            return;
        }

        var vehicle = WrapVehicle(player.CurrentVehicle);
        vehicle.EnableAllNeon();
        vehicle.SetNeonColor(0, 255, 255);
        player.SendSuccess("Неон включён.");
    }

    [Command("repaircar")]
    public void RepairCar(Client client)
    {
        var player = WrapPlayer(client);

        if (!player.IsInVehicle)
        {
            player.SendError("Ты не в машине.");
            return;
        }

        var vehicle = WrapVehicle(player.CurrentVehicle);
        vehicle.Repair();
        player.SendSuccess("Машина полностью починена.");
    }

    [Command("deletecar")]
    public void DeleteCar(Client client)
    {
        var player = WrapPlayer(client);

        if (!player.IsInVehicle)
        {
            player.SendError("Ты не в машине.");
            return;
        }

        var vehicle = WrapVehicle(player.CurrentVehicle);
        vehicle.Delete();
        player.SendMessage("~r~Машина удалена.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — забыл проверить, что игрок в машине

Неправильно:

```csharp
var vehicle = WrapVehicle(player.CurrentVehicle);
vehicle.Repair();
```

Правильно:

```csharp
if (!player.IsInVehicle)
{
    player.SendError("Ты не в машине.");
    return;
}

var vehicle = WrapVehicle(player.CurrentVehicle);
vehicle.Repair();
```

---

### Ошибка 2 — передал null в PutPlayerInto

Неправильно:

```csharp
vehicle.PutPlayerInto(null);
```

Правильно:

```csharp
if (target != null)
    vehicle.PutPlayerInto(target, VehicleSeat.FrontPassenger);
```

---

### Ошибка 3 — не проверил водителя перед использованием

Неправильно:

```csharp
var driver = vehicle.Driver;
driver.SendMessage("Привет!");
```

Правильно:

```csharp
if (vehicle.TryGetDriver(out Player driver))
{
    driver.SendMessage("Привет!");
}
```

---

### Ошибка 4 — установил слишком большой множитель мощности

Неправильно:

```csharp
vehicle.EnginePowerMultiplier = 100f;
```

Правильно:

```csharp
vehicle.EnginePowerMultiplier = 2.0f;
```

> Очень большие значения могут вызвать нестабильное поведение транспорта.

---

## Все методы Vehicle — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Delete()` | Удалить транспорт |
| `Repair()` | Починить транспорт |
| `Explode()` | Взорвать транспорт |
| `SetHealth(float)` | Установить здоровье |
| `MoveTo(Vector3)` | Переместить транспорт |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `RotateTo(Vector3)` | Повернуть транспорт |
| `RotateTo(float, float, float)` | Повернуть по координатам |
| `StartEngine()` | Запустить двигатель |
| `StopEngine()` | Заглушить двигатель |
| `Lock()` | Заблокировать |
| `Unlock()` | Разблокировать |
| `LockDoors()` | Alias для Lock() |
| `UnlockDoors()` | Alias для Unlock() |
| `SetLocked(bool)` | Заблокировать / разблокировать |
| `OpenDoor(VehicleDoor)` | Открыть дверь через enum |
| `CloseDoor(VehicleDoor)` | Закрыть дверь через enum |
| `OpenDoor(int)` | Открыть дверь по ID |
| `CloseDoor(int)` | Закрыть дверь по ID |
| `SetDoorState(VehicleDoor, bool)` | Состояние двери через enum |
| `SetDoorState(int, bool)` | Состояние двери по ID |
| `GetDoorState(VehicleDoor)` | Получить состояние двери |
| `GetDoorState(int)` | Получить состояние двери по ID |
| `IsDoorBroken(VehicleDoor)` | Сломана ли дверь |
| `IsDoorBroken(int)` | Сломана ли дверь по ID |
| `IsTyrePopped(VehicleTyre)` | Пробито ли колесо |
| `IsTyrePopped(int)` | Пробито ли колесо по ID |
| `PopTyre(VehicleTyre)` | Пробить колесо |
| `IsWindowBroken(VehicleWindow)` | Разбито ли окно |
| `IsWindowBroken(int)` | Разбито ли окно по ID |
| `GetNeonState(VehicleNeon)` | Состояние неоновой стороны |
| `GetNeonState(int)` | Состояние неоновой стороны по ID |
| `SetNeonState(VehicleNeon, bool)` | Включить / выключить сторону неона |
| `SetNeonState(int, bool)` | То же по числовому ID |
| `EnableNeon(VehicleNeon)` | Включить сторону неона |
| `DisableNeon(VehicleNeon)` | Выключить сторону неона |
| `EnableAllNeon()` | Включить весь неон |
| `DisableAllNeon()` | Выключить весь неон |
| `SetAllNeon(bool)` | Включить / выключить весь неон |
| `SetNeonColor(int, int, int)` | Установить RGB цвет неона |
| `SetColor(int, int)` | Установить оба цвета |
| `SetPrimaryColor(int)` | Установить основной цвет |
| `SetSecondaryColor(int)` | Установить вторичный цвет |
| `SetCustomPrimaryColor(Color)` | Кастомный основной RGB |
| `SetCustomPrimaryColor(int, int, int)` | Кастомный основной RGB |
| `SetCustomSecondaryColor(Color)` | Кастомный вторичный RGB |
| `SetCustomSecondaryColor(int, int, int)` | Кастомный вторичный RGB |
| `SetTyreSmokeColor(int, int, int)` | Цвет дыма из-под шин |
| `HasExtra(int)` | Проверить extra |
| `SetExtra(int, bool)` | Включить / выключить extra |
| `EnableExtra(int)` | Включить extra |
| `DisableExtra(int)` | Выключить extra |
| `GetMod(int)` | Получить мод |
| `SetMod(int, int)` | Установить мод |
| `RemoveMod(int)` | Убрать мод |
| `PutPlayerInto(Player, int)` | Посадить игрока по числу |
| `PutPlayerInto(Player, VehicleSeat)` | Посадить игрока через enum |
| `TryGetDriver(out Player)` | Безопасно получить водителя |

---

## См. также

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
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
