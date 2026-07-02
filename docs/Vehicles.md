# 🚗 Vehicles

Документация по работе с транспортом в **Glubinka MP**.

Здесь описаны функции создания, удаления, настройки, модификации и управления транспортом.

---

## Создание транспорта

### Базовое создание

```csharp
Vehicle vehicle = API.createVehicle(
    VehicleHash.Adder,               // модель
    new Vector3(0f, 0f, 72f),        // позиция
    new Vector3(0f, 0f, 0f),         // поворот
    0,                                // основной цвет
    0                                 // дополнительный цвет
);
```

---

### Создание в определённом dimension

```csharp
Vehicle vehicle = API.createVehicle(
    VehicleHash.Adder,
    new Vector3(0f, 0f, 72f),
    new Vector3(0f, 0f, 0f),
    0,
    0,
    5    // dimension
);
```

Транспорт будет виден только игрокам в dimension 5.

---

### Создание рядом с игроком

```csharp
[Command("car")]
public void CarCommand(Client player, string modelName)
{
    VehicleHash model = API.vehicleNameToModel(modelName);

    if ((int)model == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестная модель.");
        return;
    }

    Vector3 spawnPos = new Vector3(
        player.Position.X + 3f,
        player.Position.Y,
        player.Position.Z
    );

    API.createVehicle(model, spawnPos, new Vector3(0, 0, 0), 0, 0);
    API.sendChatMessageToPlayer(player, "~g~Создан: ~w~" + modelName);
}
```

---

### Создание и посадка игрока

```csharp
[Command("mycar")]
public void MyCarCommand(Client player, string modelName)
{
    VehicleHash model = API.vehicleNameToModel(modelName);

    if ((int)model == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестная модель.");
        return;
    }

    Vehicle vehicle = API.createVehicle(
        model,
        player.Position,
        new Vector3(0, 0, 0),
        0, 0
    );

    API.setPlayerIntoVehicle(player, vehicle.handle, -1);
    API.sendChatMessageToPlayer(player, "~g~Ты сел в " + modelName);
}
```

---

## Удаление транспорта

```csharp
API.deleteEntity(vehicle.handle);
```

---

## Популярные модели

### Спорткары
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Adder` | Adder |
| `VehicleHash.Zentorno` | Zentorno |
| `VehicleHash.T20` | T20 |
| `VehicleHash.Turismor` | Turismo R |
| `VehicleHash.Entityxf` | Entity XF |
| `VehicleHash.Infernus` | Infernus |
| `VehicleHash.Cheetah` | Cheetah |
| `VehicleHash.Osiris` | Osiris |
| `VehicleHash.Banshee` | Banshee |
```
---

### Мотоциклы
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Bati` | Bati 801 |
| `VehicleHash.Akuma` | Akuma |
| `VehicleHash.Hakuchou` | Hakuchou |
| `VehicleHash.Sanchez` | Sanchez |
| `VehicleHash.PCJ` | PCJ 600 |
```
---

### Грузовики
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Hauler` | Hauler |
| `VehicleHash.Phantom` | Phantom |
| `VehicleHash.Mule` | Mule |
| `VehicleHash.Benson` | Benson |
```
---

### Спецтранспорт
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Police` | Полицейская машина |
| `VehicleHash.Police2` | Полицейская машина 2 |
| `VehicleHash.Police3` | Полицейская машина 3 |
| `VehicleHash.Ambulance` | Скорая помощь |
| `VehicleHash.FireTruck` | Пожарная машина |
| `VehicleHash.Taxi` | Такси |
| `VehicleHash.Bus` | Автобус |
```
---

### Воздушный транспорт
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Buzzard2` | Buzzard |
| `VehicleHash.Maverick` | Maverick |
| `VehicleHash.Lazer` | Lazer (истребитель) |
| `VehicleHash.Hydra` | Hydra |
| `VehicleHash.Titan` | Titan |
| `VehicleHash.Luxor` | Luxor |
```
---

### Водный транспорт
```
| VehicleHash | Название |
|---|---|
| `VehicleHash.Jetmax` | Jetmax |
| `VehicleHash.Speeder` | Speeder |
| `VehicleHash.Dinghy` | Dinghy |
| `VehicleHash.Marquis` | Marquis |
```
---

## Здоровье транспорта

### Получить здоровье

```csharp
float health = API.getVehicleHealth(vehicle.handle);
```

---

### Установить здоровье

```csharp
API.setVehicleHealth(vehicle.handle, 1000f);
```

> Максимальное здоровье = `1000`. При `0` и ниже транспорт уничтожается.

---

### Починить транспорт

```csharp
API.repairVehicle(vehicle.handle);
```

> Это восстанавливает здоровье до 1000 и чинит визуальные повреждения.

---

### Взорвать транспорт

```csharp
API.explodeVehicle(vehicle.handle);
```

---

## Цвета

### Установить основной цвет (по ID)

```csharp
API.setVehiclePrimaryColor(vehicle.handle, 27);
```

---

### Установить дополнительный цвет (по ID)

```csharp
API.setVehicleSecondaryColor(vehicle.handle, 12);
```

---

### Установить основной цвет (RGB)

```csharp
API.setVehicleCustomPrimaryColor(vehicle.handle, 255, 0, 0);
```

---

### Установить дополнительный цвет (RGB)

```csharp
API.setVehicleCustomSecondaryColor(vehicle.handle, 0, 0, 255);
```

---

### Получить основной цвет

```csharp
int color = API.getVehiclePrimaryColor(vehicle.handle);
```

---

### Получить дополнительный цвет

```csharp
int color = API.getVehicleSecondaryColor(vehicle.handle);
```

---

### Получить основной цвет (RGB)

```csharp
Color color = API.getVehicleCustomPrimaryColor(vehicle.handle);
// color.red, color.green, color.blue
```

---

### Получить дополнительный цвет (RGB)

```csharp
Color color = API.getVehicleCustomSecondaryColor(vehicle.handle);
```

---

### Популярные ID цветов
```
| ID | Цвет |
|---|---|
| 0 | Чёрный |
| 1 | Графит |
| 2 | Тёмно-серебряный |
| 3 | Серебряный |
| 12 | Красный |
| 27 | Жёлтый |
| 38 | Синий |
| 51 | Зелёный |
| 111 | Оранжевый |
| 134 | Белый |
```
---

## Номерные знаки

### Установить текст номера

```csharp
API.setVehicleNumberPlate(vehicle.handle, "GLBNK MP");
```

---

### Получить текст номера

```csharp
string plate = API.getVehicleNumberPlate(vehicle.handle);
```

---

### Установить стиль номера

```csharp
API.setVehicleNumberPlateStyle(vehicle.handle, 1);
```

---

### Получить стиль номера

```csharp
int style = API.getVehicleNumberPlateStyle(vehicle.handle);
```

---

## Замки

### Заблокировать двери

```csharp
API.setVehicleLocked(vehicle.handle, true);
```

---

### Разблокировать двери

```csharp
API.setVehicleLocked(vehicle.handle, false);
```

---

### Проверить заблокирован ли

```csharp
bool locked = API.getVehicleLocked(vehicle.handle);
```

---

## Двигатель

### Включить двигатель

```csharp
API.setVehicleEngineStatus(vehicle.handle, true);
```

---

### Выключить двигатель

```csharp
API.setVehicleEngineStatus(vehicle.handle, false);
```

---

### Проверить состояние двигателя

```csharp
bool engineOn = API.getVehicleEngineStatus(vehicle.handle);
```

---

## Двери

### Открыть дверь

```csharp
API.setVehicleDoorState(vehicle.handle, 0, true);
```

---

### Закрыть дверь

```csharp
API.setVehicleDoorState(vehicle.handle, 0, false);
```

---

### Проверить открыта ли дверь

```csharp
bool open = API.getVehicleDoorState(vehicle.handle, 0);
```

---

### Сломать дверь

```csharp
API.breakVehicleDoor(vehicle.handle, 0, true);
```

---

### Починить дверь

```csharp
API.breakVehicleDoor(vehicle.handle, 0, false);
```

---

### Проверить сломана ли дверь

```csharp
bool broken = API.isVehicleDoorBroken(vehicle.handle, 0);
```

---

### Индексы дверей
```
| Индекс | Дверь |
|---|---|
| 0 | Передняя левая |
| 1 | Передняя правая |
| 2 | Задняя левая |
| 3 | Задняя правая |
| 4 | Капот |
| 5 | Багажник |
```
---

## Стёкла

### Разбить стекло

```csharp
API.breakVehicleWindow(vehicle.handle, 0, true);
```

---

### Починить стекло

```csharp
API.breakVehicleWindow(vehicle.handle, 0, false);
```

---

### Проверить разбито ли стекло

```csharp
bool broken = API.isVehicleWindowBroken(vehicle.handle, 0);
```

---

### Индексы стёкол
```
| Индекс | Стекло |
|---|---|
| 0 | Переднее левое |
| 1 | Переднее правое |
| 2 | Заднее левое |
| 3 | Заднее правое |
```
---

## Колёса

### Проколоть колесо

```csharp
API.popVehicleTyre(vehicle.handle, 0, true);
```

---

### Починить колесо

```csharp
API.popVehicleTyre(vehicle.handle, 0, false);
```

---

### Проверить проколото ли колесо

```csharp
bool popped = API.isVehicleTyrePopped(vehicle.handle, 0);
```

---

### Индексы колёс
```
| Индекс | Колесо |
|---|---|
| 0 | Переднее левое |
| 1 | Переднее правое |
| 2 | Заднее левое |
| 3 | Заднее правое |
| 4 | Запасное переднее (если есть) |
| 5 | Запасное заднее (если есть) |
```
---

### Бронированные колёса

```csharp
API.setVehicleBulletproofTyres(vehicle.handle, true);
```

```csharp
bool bulletproof = API.getVehicleBulletproofTyres(vehicle.handle);
```

---

## Модификации

### Установить мод

```csharp
API.setVehicleMod(vehicle.handle, modType, modIndex);
```

---

### Получить мод

```csharp
int mod = API.getVehicleMod(vehicle.handle, modType);
```

---

### Удалить мод

```csharp
API.removeVehicleMod(vehicle.handle, modType);
```

---

### Типы модов
```
| modType | Описание |
|---|---|
| 0 | Спойлер |
| 1 | Передний бампер |
| 2 | Задний бампер |
| 3 | Боковые юбки |
| 4 | Выхлопная система |
| 5 | Каркас безопасности |
| 6 | Решётка |
| 7 | Капот |
| 8 | Крыло |
| 10 | Крыша |
| 11 | Двигатель |
| 12 | Тормоза |
| 13 | Трансмиссия |
| 14 | Клаксон |
| 15 | Подвеска |
| 16 | Броня |
| 22 | Ксенон |
| 23 | Колёса (тип) |
| 24 | Задние колёса (для мотоциклов) |
```
---

## Ливреи

### Установить ливрею

```csharp
API.setVehicleLivery(vehicle.handle, 0);
```

---

### Получить ливрею

```csharp
int livery = API.getVehicleLivery(vehicle.handle);
```

---

## Неон

### Включить неон

```csharp
API.setVehicleNeonState(vehicle.handle, 0, true);  // левый
API.setVehicleNeonState(vehicle.handle, 1, true);  // правый
API.setVehicleNeonState(vehicle.handle, 2, true);  // передний
API.setVehicleNeonState(vehicle.handle, 3, true);  // задний
```

---

### Выключить неон

```csharp
API.setVehicleNeonState(vehicle.handle, 0, false);
```

---

### Проверить неон

```csharp
bool neon = API.getVehicleNeonState(vehicle.handle, 0);
```

---

### Установить цвет неона

```csharp
API.setVehicleNeonColor(vehicle.handle, 255, 0, 255);
```

---

### Получить цвет неона

```csharp
Color neonColor = API.getVehicleNeonColor(vehicle.handle);
```

---

## Дым из-под колёс

### Установить цвет дыма

```csharp
API.setVehicleTyreSmokeColor(vehicle.handle, 255, 0, 0);
```

---

### Получить цвет дыма

```csharp
Color smoke = API.getVehicleTyreSmokeColor(vehicle.handle);
```

---

## Тонировка стёкол

### Установить тонировку

```csharp
API.setVehicleWindowTint(vehicle.handle, 1);
```
```
| ID | Тип |
|---|---|
| 0 | Без тонировки |
| 1 | Чёрная |
| 2 | Тёмная |
| 3 | Светлая |
```
---

### Получить тонировку

```csharp
int tint = API.getVehicleWindowTint(vehicle.handle);
```

---

## Мощность и крутящий момент

### Установить множитель мощности

```csharp
API.setVehicleEnginePowerMultiplier(vehicle.handle, 5f);
```

---

### Получить множитель мощности

```csharp
float power = API.getVehicleEnginePowerMultiplier(vehicle.handle);
```

---

### Установить множитель крутящего момента

```csharp
API.setVehicleEngineTorqueMultiplier(vehicle.handle, 5f);
```

---

### Получить множитель крутящего момента

```csharp
float torque = API.getVehicleEngineTorqueMultiplier(vehicle.handle);
```

---

## Спецсвет

### Включить спецсвет (такси)

```csharp
API.setVehicleSpecialLightStatus(vehicle.handle, true);
```

---

### Выключить спецсвет

```csharp
API.setVehicleSpecialLightStatus(vehicle.handle, false);
```

---

## Extra (дополнительные элементы)

### Включить extra

```csharp
API.setVehicleExtra(vehicle.handle, 0, true);
```

---

### Выключить extra

```csharp
API.setVehicleExtra(vehicle.handle, 0, false);
```

---

### Проверить extra

```csharp
bool extra = API.getVehicleExtra(vehicle.handle, 0);
```

---

## Сирена

### Проверить сирену

```csharp
bool siren = API.getVehicleSirenState(vehicle.handle);
```

---

## Прицепы

### Получить прицеп

```csharp
NetHandle trailer = API.getVehicleTrailer(vehicle.handle);
```

---

### Получить тягач

```csharp
NetHandle tower = API.getVehicleTraileredBy(vehicle.handle);
```

---

## Пассажиры

### Получить всех пассажиров

```csharp
Client[] occupants = API.getVehicleOccupants(vehicle.handle);

foreach (Client p in occupants)
{
    API.consoleOutput("Пассажир: " + p.Name);
}
```

---

### Получить водителя

```csharp
Client driver = API.getVehicleDriver(vehicle.handle);

if (driver != null)
{
    API.consoleOutput("Водитель: " + driver.Name);
}
```

---

### Посадить игрока

```csharp
API.setPlayerIntoVehicle(player, vehicle.handle, -1);
```
```
| Seat | Описание |
|---|---|
| `-1` | Водитель |
| `0` | Передний пассажир |
| `1` | Задний левый |
| `2` | Задний правый |
```
---

### Высадить игрока

```csharp
API.warpPlayerOutOfVehicle(player);
```

---

## Позиция и поворот

### Получить позицию

```csharp
Vector3 pos = API.getEntityPosition(vehicle.handle);
```

---

### Установить позицию

```csharp
API.setEntityPosition(vehicle.handle, new Vector3(100f, 200f, 30f));
```

---

### Получить поворот

```csharp
Vector3 rot = API.getEntityRotation(vehicle.handle);
```

---

### Установить поворот

```csharp
API.setEntityRotation(vehicle.handle, new Vector3(0f, 0f, 90f));
```

---

## Неуязвимость и столкновения

### Сделать неуязвимым

```csharp
API.setEntityInvincible(vehicle.handle, true);
```

---

### Убрать неуязвимость

```csharp
API.setEntityInvincible(vehicle.handle, false);
```

---

### Отключить столкновения

```csharp
API.setEntityCollisionless(vehicle.handle, true);
```

---

### Включить столкновения

```csharp
API.setEntityCollisionless(vehicle.handle, false);
```

---

## Прозрачность

### Установить прозрачность

```csharp
API.setEntityTransparency(vehicle.handle, 100);
```

> Значение от 0 (невидимый) до 255 (полностью видимый).

---

### Получить прозрачность

```csharp
byte alpha = API.getEntityTransparency(vehicle.handle);
```

---

## Информация о моделях

### Название модели

```csharp
string name = API.getVehicleDisplayName(VehicleHash.Adder);
```

---

### Максимальная скорость

```csharp
float speed = API.getVehicleMaxSpeed(VehicleHash.Adder);
```

---

### Максимальное торможение

```csharp
float braking = API.getVehicleMaxBraking(VehicleHash.Adder);
```

---

### Максимальное сцепление

```csharp
float traction = API.getVehicleMaxTraction(VehicleHash.Adder);
```

---

### Максимальное ускорение

```csharp
float accel = API.getVehicleMaxAcceleration(VehicleHash.Adder);
```

---

### Максимум пассажиров

```csharp
float seats = API.getVehicleMaxOccupants(VehicleHash.Adder);
```

---

### Класс транспорта

```csharp
int classId = API.getVehicleClass(VehicleHash.Adder);
string className = API.getVehicleClassName(classId);
```

---

## Получение списков

### Все транспортные средства

```csharp
List<NetHandle> allVehicles = API.getAllVehicles();

foreach (NetHandle v in allVehicles)
{
    API.consoleOutput("Vehicle: " + v.Value);
}
```

---

## Пример: полный набор команд

```csharp
using GTANetworkServer;
using GTANetworkShared;

public class VehicleCommands : Script
{
    [Command("veh")]
    public void SpawnVehicleCommand(Client player, string modelName)
    {
        VehicleHash model = API.vehicleNameToModel(modelName);
        if ((int)model == 0)
        {
            API.sendChatMessageToPlayer(player, "~r~Неизвестная модель.");
            return;
        }

        Vehicle vehicle = API.createVehicle(model, player.Position, new Vector3(0,0,0), 0, 0);
        API.setPlayerIntoVehicle(player, vehicle.handle, -1);
        API.sendChatMessageToPlayer(player, "~g~Создан: ~w~" + modelName);
    }

    [Command("repair")]
    public void RepairCommand(Client player)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        API.repairVehicle(player.CurrentVehicle);
        API.sendChatMessageToPlayer(player, "~g~Транспорт починен.");
    }

    [Command("vcolor")]
    public void ColorCommand(Client player, int color1, int color2)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        API.setVehiclePrimaryColor(player.CurrentVehicle, color1);
        API.setVehicleSecondaryColor(player.CurrentVehicle, color2);
        API.sendChatMessageToPlayer(player, "~g~Цвет изменён.");
    }

    [Command("vlock")]
    public void LockCommand(Client player)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        bool locked = API.getVehicleLocked(player.CurrentVehicle);
        API.setVehicleLocked(player.CurrentVehicle, !locked);
        API.sendChatMessageToPlayer(player, locked ? "~g~Разблокировано." : "~r~Заблокировано.");
    }

    [Command("vplate")]
    public void PlateCommand(Client player, string text)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        API.setVehicleNumberPlate(player.CurrentVehicle, text);
        API.sendChatMessageToPlayer(player, "~g~Номер: ~w~" + text);
    }

    [Command("vneon")]
    public void NeonCommand(Client player, int r, int g, int b)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        NetHandle veh = player.CurrentVehicle;
        API.setVehicleNeonState(veh, 0, true);
        API.setVehicleNeonState(veh, 1, true);
        API.setVehicleNeonState(veh, 2, true);
        API.setVehicleNeonState(veh, 3, true);
        API.setVehicleNeonColor(veh, r, g, b);
        API.sendChatMessageToPlayer(player, "~g~Неон включён.");
    }

    [Command("vdel")]
    public void DeleteCommand(Client player)
    {
        if (!player.IsInVehicle)
        {
            API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
            return;
        }

        NetHandle veh = player.CurrentVehicle;
        API.warpPlayerOutOfVehicle(player);
        API.deleteEntity(veh);
        API.sendChatMessageToPlayer(player, "~r~Транспорт удалён.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — не проверил модель

Неправильно:
```csharp
VehicleHash model = API.vehicleNameToModel(name);
API.createVehicle(model, pos, rot, 0, 0);
```

Правильно:
```csharp
VehicleHash model = API.vehicleNameToModel(name);
if ((int)model == 0)
{
    API.sendChatMessageToPlayer(player, "~r~Неизвестная модель.");
    return;
}
API.createVehicle(model, pos, rot, 0, 0);
```

---

### Ошибка 2 — не проверил что игрок в транспорте

Неправильно:
```csharp
API.repairVehicle(player.CurrentVehicle);
```

Правильно:
```csharp
if (!player.IsInVehicle)
{
    API.sendChatMessageToPlayer(player, "~r~Ты не в транспорте.");
    return;
}
API.repairVehicle(player.CurrentVehicle);
```

---

### Ошибка 3 — забыл высадить игрока перед удалением

Неправильно:
```csharp
API.deleteEntity(player.CurrentVehicle);
```

Правильно:
```csharp
API.warpPlayerOutOfVehicle(player);
API.deleteEntity(player.CurrentVehicle);
```

---

### Ошибка 4 — спавн транспорта на той же позиции что и игрок

Если заспавнить транспорт точно на позиции игрока, он может "войти" в модель.

Лучше добавить смещение:

```csharp
Vector3 spawnPos = new Vector3(
    player.Position.X + 3f,
    player.Position.Y,
    player.Position.Z
);
```

---

## См. также

- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
