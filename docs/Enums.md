# Enums

Общий справочник enum'ов в **Glubinka MP SDK**.

Этот файл собирает в одном месте все публичные enum'ы SDK, чтобы разработчику ресурса не приходилось помнить числовые значения вручную.

---

## Что такое enum

`enum` — это набор именованных значений.

Они нужны для того, чтобы вместо непонятных чисел писать понятные имена.

Неправильно:

```csharp
vehicle.OpenDoor((VehicleDoor)5);
player.PutIntoVehicle(vehicle, (VehicleSeat)(-1));
World.SetWeather((WeatherType)6);
```

Правильно:

```csharp
vehicle.OpenDoor(VehicleDoor.Hood);
player.PutIntoVehicle(vehicle, VehicleSeat.Driver);
World.SetWeather(WeatherType.Rain);
```

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## VehicleDoor

`VehicleDoor` — двери транспорта.

Используется в `Vehicle.OpenDoor`, `Vehicle.CloseDoor`, `Vehicle.SetDoorState`, `Vehicle.GetDoorState`, `Vehicle.IsDoorBroken`.

### Значения

| Значение | Число | Описание |
|---|---|---|
| `VehicleDoor.FrontLeft` | 0 | Передняя левая дверь (водительская) |
| `VehicleDoor.FrontRight` | 1 | Передняя правая дверь |
| `VehicleDoor.RearLeft` | 2 | Задняя левая дверь |
| `VehicleDoor.RearRight` | 3 | Задняя правая дверь |
| `VehicleDoor.Trunk` | 4 | Багажник |
| `VehicleDoor.Hood` | 5 | Капот |

### Пример

```csharp
vehicle.OpenDoor(VehicleDoor.Hood);
vehicle.OpenDoor(VehicleDoor.Trunk);
vehicle.CloseDoor(VehicleDoor.FrontLeft);
```

---

## VehicleTyre

`VehicleTyre` — колёса транспорта.

Используется в `Vehicle.IsTyrePopped` и `Vehicle.PopTyre`.

### Значения

| Значение | Число | Описание |
|---|---|---|
| `VehicleTyre.FrontLeft` | 0 | Переднее левое колесо |
| `VehicleTyre.FrontRight` | 1 | Переднее правое колесо |
| `VehicleTyre.MiddleLeft` | 2 | Среднее левое колесо |
| `VehicleTyre.MiddleRight` | 3 | Среднее правое колесо |
| `VehicleTyre.RearLeft` | 4 | Заднее левое колесо |
| `VehicleTyre.RearRight` | 5 | Заднее правое колесо |

### Пример

```csharp
vehicle.PopTyre(VehicleTyre.FrontLeft);

if (vehicle.IsTyrePopped(VehicleTyre.RearRight))
{
    player.SendWarning("Заднее правое колесо пробито.");
}
```

---

## VehicleWindow

`VehicleWindow` — окна транспорта.

Используется в `Vehicle.IsWindowBroken`.

### Значения

| Значение | Число | Описание |
|---|---|---|
| `VehicleWindow.FrontLeft` | 0 | Переднее левое окно |
| `VehicleWindow.FrontRight` | 1 | Переднее правое окно |
| `VehicleWindow.RearLeft` | 2 | Заднее левое окно |
| `VehicleWindow.RearRight` | 3 | Заднее правое окно |

### Пример

```csharp
bool broken = vehicle.IsWindowBroken(VehicleWindow.FrontLeft);

if (broken)
{
    player.SendInfo("Водительское окно разбито.");
}
```

---

## VehicleNeon

`VehicleNeon` — стороны неона транспорта.

Используется в `Vehicle.GetNeonState`, `Vehicle.SetNeonState`, `Vehicle.EnableNeon`, `Vehicle.DisableNeon`.

### Значения

| Значение | Число | Описание |
|---|---|---|
| `VehicleNeon.Left` | 0 | Левый неон |
| `VehicleNeon.Right` | 1 | Правый неон |
| `VehicleNeon.Front` | 2 | Передний неон |
| `VehicleNeon.Back` | 3 | Задний неон |

### Пример

```csharp
vehicle.EnableNeon(VehicleNeon.Left);
vehicle.EnableNeon(VehicleNeon.Right);
vehicle.SetNeonState(VehicleNeon.Front, true);
```

---

## VehicleSeat

`VehicleSeat` — места в транспорте.

Используется в:
- `Player.PutIntoVehicle(vehicle, VehicleSeat seat)`
- `Vehicle.PutPlayerInto(player, VehicleSeat seat)`

### Значения

| Значение | Число | Описание |
|---|---|---|
| `VehicleSeat.Any` | -2 | Любое свободное место |
| `VehicleSeat.Driver` | -1 | Водитель |
| `VehicleSeat.FrontPassenger` | 0 | Передний пассажир |
| `VehicleSeat.RearLeft` | 1 | Заднее левое место |
| `VehicleSeat.RearRight` | 2 | Заднее правое место |
| `VehicleSeat.Extra1` | 3 | Дополнительное место 1 |
| `VehicleSeat.Extra2` | 4 | Дополнительное место 2 |

### Пример

```csharp
player.PutIntoVehicle(vehicle, VehicleSeat.Driver);
vehicle.PutPlayerInto(targetPlayer, VehicleSeat.FrontPassenger);
```

---

## ClothesSlot

`ClothesSlot` — слоты одежды персонажа.

Используется в:
- `Player.SetClothes(ClothesSlot, int, int)`
- `Player.GetClothesDrawable(ClothesSlot)`
- `Player.GetClothesTexture(ClothesSlot)`

### Значения

| Значение | Число | Описание |
|---|---|---|
| `ClothesSlot.Head` | 0 | Голова |
| `ClothesSlot.Mask` | 1 | Маска / лицо |
| `ClothesSlot.Torso` | 3 | Верхняя одежда |
| `ClothesSlot.Legs` | 4 | Штаны / низ |
| `ClothesSlot.Bags` | 5 | Сумки / парашют |
| `ClothesSlot.Shoes` | 6 | Обувь |
| `ClothesSlot.Accessories` | 7 | Шарфы / цепи |
| `ClothesSlot.Undershirt` | 8 | Рубашка / футболка |
| `ClothesSlot.BodyArmor` | 9 | Бронежилет |
| `ClothesSlot.Decals` | 10 | Значки / декали |
| `ClothesSlot.Tops` | 11 | Верхняя одежда поверх |

### Пример

```csharp
player.SetClothes(ClothesSlot.Tops, 15, 0);
player.SetClothes(ClothesSlot.Legs, 10, 0);
player.SetClothes(ClothesSlot.Shoes, 3, 0);
```

---

## AccessorySlot

`AccessorySlot` — слоты аксессуаров персонажа.

Используется в:
- `Player.SetAccessory(AccessorySlot, int, int)`
- `Player.ClearAccessory(AccessorySlot)`
- `Player.GetAccessoryDrawable(AccessorySlot)`
- `Player.GetAccessoryTexture(AccessorySlot)`

### Значения

| Значение | Число | Описание |
|---|---|---|
| `AccessorySlot.Hat` | 0 | Головной убор |
| `AccessorySlot.Glasses` | 1 | Очки |
| `AccessorySlot.Ears` | 2 | Уши / наушники / серьги |
| `AccessorySlot.Watch` | 6 | Часы |
| `AccessorySlot.Bracelet` | 7 | Браслет |

### Пример

```csharp
player.SetAccessory(AccessorySlot.Hat, 45, 0);
player.SetAccessory(AccessorySlot.Glasses, 5, 0);
player.ClearAccessory(AccessorySlot.Watch);
```

---

## ColShapeType

`ColShapeType` — тип колшейпа.

Используется в свойстве `ColShape.Type` и в проверках:
- `IsSphere`
- `IsCylinder`
- `IsRectangle2D`
- `IsRectangle3D`

### Значения

| Значение | Число | Описание |
|---|---|---|
| `ColShapeType.Sphere` | 0 | Сфера |
| `ColShapeType.Cylinder` | 1 | Цилиндр |
| `ColShapeType.Rectangle2D` | 2 | 2D прямоугольник |
| `ColShapeType.Rectangle3D` | 3 | 3D прямоугольник |

### Пример

```csharp
if (zone.Type == ColShapeType.Sphere)
{
    Log("Это сферический колшейп.");
}

if (zone.IsRectangle3D)
{
    Log("Это 3D прямоугольная зона.");
}
```

---

## WeatherType

`WeatherType` — тип погоды в мире.

Используется в:
- `World.Weather`
- `World.SetWeather(WeatherType)`
- `World.SetWeatherAndTime(WeatherType, int, int)`

### Значения

| Значение | Число | Описание |
|---|---|---|
| `WeatherType.ExtraSunny` | 0 | Очень солнечно |
| `WeatherType.Clear` | 1 | Ясно |
| `WeatherType.Clouds` | 2 | Облачно |
| `WeatherType.Smog` | 3 | Смог |
| `WeatherType.Foggy` | 4 | Туман |
| `WeatherType.Overcast` | 5 | Пасмурно |
| `WeatherType.Rain` | 6 | Дождь |
| `WeatherType.Thunder` | 7 | Гроза |
| `WeatherType.Clearing` | 8 | Прояснение |
| `WeatherType.Neutral` | 9 | Нейтральная погода |
| `WeatherType.Snow` | 10 | Снег |
| `WeatherType.Blizzard` | 11 | Метель |
| `WeatherType.Snowlight` | 12 | Лёгкий снег |
| `WeatherType.Xmas` | 13 | Рождественская погода |

### Пример

```csharp
World.SetWeather(WeatherType.Rain);
World.SetWeatherAndTime(WeatherType.Thunder, 2, 0);
```

---

## LogLevel

`LogLevel` — уровень логирования.

Используется внутри `Logger`, а также может использоваться как справочный enum в твоём коде.

### Значения

| Значение | Число | Описание |
|---|---|---|
| `LogLevel.Debug` | 0 | Отладочная информация |
| `LogLevel.Info` | 1 | Обычная информация |
| `LogLevel.Warning` | 2 | Предупреждение |
| `LogLevel.Error` | 3 | Ошибка |

### Пример

```csharp
LogLevel level = LogLevel.Warning;

if (level == LogLevel.Warning)
{
    log.Warning("Это предупреждение.");
}
```

> Обычно напрямую используется сам `Logger`, а не `LogLevel`.

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class EnumsDemo : GlubinkaContext
{
    [Command("enumdemo")]
    public void EnumDemo(Client client)
    {
        var player = WrapPlayer(client);
        var vehicle = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));

        // Погода
        World.SetWeather(WeatherType.Clear);

        // Транспорт
        player.PutIntoVehicle(vehicle, VehicleSeat.Driver);
        vehicle.OpenDoor(VehicleDoor.Hood);
        vehicle.EnableNeon(VehicleNeon.Left);
        vehicle.EnableNeon(VehicleNeon.Right);
        vehicle.PopTyre(VehicleTyre.FrontLeft);

        // Одежда
        player.SetClothes(ClothesSlot.Tops, 15, 0);
        player.SetAccessory(AccessorySlot.Glasses, 5, 0);

        player.SendSuccess("Пример enum-ов выполнен.");
    }

    protected override void OnStart()
    {
        var zone = CreateSphereColShape(new Vector3(0, 0, 72), 3f);

        if (zone.Type == ColShapeType.Sphere)
        {
            Log("Создан сферический колшейп.");
        }
    }
}
```

---

## Частые ошибки

### Ошибка 1 — используешь числа вместо enum

Неправильно:

```csharp
vehicle.OpenDoor((VehicleDoor)5);
player.PutIntoVehicle(vehicle, (VehicleSeat)(-1));
World.SetWeather((WeatherType)6);
```

Правильно:

```csharp
vehicle.OpenDoor(VehicleDoor.Hood);
player.PutIntoVehicle(vehicle, VehicleSeat.Driver);
World.SetWeather(WeatherType.Rain);
```

---

### Ошибка 2 — используешь несуществующее значение enum

Неправильно:

```csharp
player.PutIntoVehicle(vehicle, VehicleSeat.Passenger);
```

Правильно:

```csharp
player.PutIntoVehicle(vehicle, VehicleSeat.FrontPassenger);
```

---

### Ошибка 3 — путаешь ClothesSlot и AccessorySlot

Неправильно:

```csharp
player.SetClothes(AccessorySlot.Hat, 45, 0);
```

Правильно:

```csharp
player.SetAccessory(AccessorySlot.Hat, 45, 0);
```

Или для одежды:

```csharp
player.SetClothes(ClothesSlot.Tops, 15, 0);
```

---

### Ошибка 4 — передаёшь enum не в тот метод

Неправильно:

```csharp
vehicle.PopTyre(VehicleDoor.Hood);
```

Правильно:

```csharp
vehicle.PopTyre(VehicleTyre.FrontLeft);
```

---

### Ошибка 5 — забыл подключить namespace SDK

Неправильно:

```csharp
public class MyResource : GlubinkaContext
{
    // enum'ы не видны
}
```

Правильно:

```csharp
using GlubinkaMP.SDK;
```

---

## Все enum'ы SDK — сводная таблица

| Enum | Для чего нужен |
|---|---|
| `VehicleDoor` | Двери транспорта |
| `VehicleTyre` | Колёса транспорта |
| `VehicleWindow` | Окна транспорта |
| `VehicleNeon` | Стороны неона транспорта |
| `VehicleSeat` | Сиденья транспорта |
| `ClothesSlot` | Слоты одежды персонажа |
| `AccessorySlot` | Слоты аксессуаров персонажа |
| `ColShapeType` | Типы колшейпов |
| `WeatherType` | Типы погоды |
| `LogLevel` | Уровни логирования |

---

## См. также
