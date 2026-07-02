# 🌍 World

Документация по управлению игровым миром в **Glubinka MP**.

Здесь описаны функции для управления погодой, временем суток, IPL (интерьеры), измерениями (dimensions), данными мира и другими глобальными параметрами.

---

## Погода

### Установить погоду

```csharp
API.setWeather(1);
```

> Допустимые значения: от 0 до 13.  
> Значения вне диапазона могут вызвать проблемы.

---

### Получить текущую погоду

```csharp
int weather = API.getWeather();
API.consoleOutput("Текущая погода: " + weather);
```

---

### Таблица ID погоды
```
| ID | Название | Описание |
|---|---|---|
| 0 | EXTRASUNNY | Очень солнечно |
| 1 | CLEAR | Ясно |
| 2 | CLOUDS | Облачно |
| 3 | SMOG | Смог |
| 4 | FOGGY | Туман |
| 5 | OVERCAST | Пасмурно |
| 6 | RAIN | Дождь |
| 7 | THUNDER | Гроза |
| 8 | CLEARING | Проясняется |
| 9 | NEUTRAL | Нейтрально |
| 10 | SNOW | Снег |
| 11 | BLIZZARD | Метель |
| 12 | SNOWLIGHT | Лёгкий снег |
| 13 | XMAS | Рождество |
```
---

### Пример команды погоды

```csharp
[Command("weather")]
public void WeatherCommand(Client player, int id)
{
    if (id < 0 || id > 13)
    {
        API.sendChatMessageToPlayer(player, "~r~Погода должна быть от 0 до 13.");
        return;
    }

    API.setWeather(id);
    API.sendChatMessageToAll("~b~Погода изменена на ID: ~w~" + id);
}
```

---

## Время суток

### Установить время

```csharp
API.setTime(12, 30);
```

Параметры:
```
| Параметр | Тип | Диапазон | Описание |
|---|---|---|---|
| `hours` | `int` | 0–23 | Часы |
| `minutes` | `int` | 0–59 | Минуты |
```
---

### Получить текущее время

```csharp
TimeSpan time = API.getTime();
API.consoleOutput("Время: " + time.Hours + ":" + time.Minutes);
```

---

### Заморозить время для игрока

```csharp
API.freezePlayerTime(player, true);
```

Это остановит смену времени только для этого игрока.

---

### Разморозить время для игрока

```csharp
API.freezePlayerTime(player, false);
```

---

### Пример команды времени

```csharp
[Command("time")]
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

---

### Популярные значения времени
```
| Часы | Описание |
|---|---|
| 0 | Полночь |
| 6 | Рассвет |
| 12 | Полдень |
| 18 | Закат |
| 21 | Ночь |
```
---

## IPL (интерьеры и локации)

IPL — это дополнительные части карты, которые можно загружать и выгружать.

Например: интерьеры зданий, разрушенные версии локаций, праздничные декорации.

---

### Загрузить IPL

```csharp
API.requestIpl("ex_dt1_02_intoo_01");
```

---

### Выгрузить IPL

```csharp
API.removeIpl("ex_dt1_02_intoo_01");
```

---

### Сбросить все IPL

```csharp
API.resetIplList();
```

---

### Популярные IPL
```
| IPL | Описание |
|---|---|
| `ex_dt1_02_intoo_01` | Офис FIB |
| `chop_props` | Двор Франклина |
| `FIBlobby` | Лобби FIB |
| `FBI_interior` | Интерьер ФБР |
| `CS1_02_cf_onmission1` | Ювелирный магазин (до ограбления) |
| `CS1_02_cf_onmission2` | Ювелирный магазин (после ограбления) |
| `v_19_carshowroom` | Автосалон |
| `v_casino` | Казино |
| `hei_bi_hw1_13_door` | Дверь Humane Labs |
```
---

### Пример: загрузить интерьер при старте

```csharp
public class MyResource : Script
{
    public MyResource()
    {
        API.onResourceStart += OnResourceStart;
    }

    private void OnResourceStart()
    {
        API.requestIpl("ex_dt1_02_intoo_01");
        API.consoleOutput("IPL загружен: ex_dt1_02_intoo_01");
    }
}
```

---

## Dimensions (измерения)

Dimensions позволяют разделять игроков по виртуальным мирам.

Игроки в разных dimensions не видят друг друга, не видят транспорт и объекты из других dimensions.

---

### Установить dimension для сущности

```csharp
API.setEntityDimension(player.handle, 1);
```

---

### Получить dimension сущности

```csharp
int dim = API.getEntityDimension(player.handle);
API.consoleOutput("Dimension: " + dim);
```

---

### Dimension 0

Dimension `0` — это глобальный мир.

Все сущности по умолчанию находятся в dimension `0`.

Сущности в dimension `0` видны всем.

---

### Пример: отдельная комната для игрока

```csharp
private int _nextRoom = 1;

[Command("privateroom")]
public void PrivateRoomCommand(Client player)
{
    int room = _nextRoom;
    _nextRoom++;

    API.setEntityDimension(player.handle, room);
    API.setEntityPosition(player.handle, new Vector3(0f, 0f, 72f));
    API.sendChatMessageToPlayer(player, "~g~Ты перемещён в комнату #" + room);
}

[Command("globalroom")]
public void GlobalRoomCommand(Client player)
{
    API.setEntityDimension(player.handle, 0);
    API.sendChatMessageToPlayer(player, "~g~Ты вернулся в глобальный мир.");
}
```

---

### Пример: dimension для транспорта

```csharp
NetHandle vehicle = API.createVehicle(
    VehicleHash.Adder,
    new Vector3(0f, 0f, 72f),
    new Vector3(0, 0, 0),
    0, 0,
    5  // dimension 5
).handle;
```

Этот транспорт будет виден только игрокам в dimension 5.

---

## Данные мира (World Data)

Глобальные данные которые не привязаны к конкретной сущности.

---

### Серверные данные мира (не синхронизируются)

Только сервер может читать и писать. Клиенты не видят.

#### Записать

```csharp
API.setWorldData("serverStartTime", DateTime.Now.ToString());
```

---

#### Прочитать

```csharp
string startTime = API.getWorldData("serverStartTime");
```

---

#### Проверить наличие

```csharp
bool exists = API.hasWorldData("serverStartTime");
```

---

#### Удалить

```csharp
API.resetWorldData("serverStartTime");
```

---

#### Получить все ключи

```csharp
string[] allKeys = API.getAllWorldData();

foreach (string key in allKeys)
{
    API.consoleOutput("World data key: " + key);
}
```

---

### Синхронизированные данные мира

Эти данные видны и серверу и всем клиентам.

#### Записать

```csharp
API.setWorldSyncedData("gameMode", "Deathmatch");
```

---

#### Прочитать

```csharp
string mode = API.getWorldSyncedData("gameMode");
```

---

#### Проверить наличие

```csharp
bool exists = API.hasWorldSyncedData("gameMode");
```

---

#### Удалить

```csharp
API.resetWorldSyncedData("gameMode");
```

---

#### Получить все ключи

```csharp
string[] allKeys = API.getAllWorldSyncedData();
```

---

## Данные сущностей (Entity Data)

Данные привязанные к конкретной сущности (игрок, транспорт, объект).

---

### Серверные данные сущности

#### Записать

```csharp
API.setEntityData(player.handle, "score", 100);
```

---

#### Прочитать

```csharp
int score = API.getEntityData(player.handle, "score");
```

---

#### Проверить

```csharp
bool has = API.hasEntityData(player.handle, "score");
```

---

#### Удалить

```csharp
API.resetEntityData(player.handle, "score");
```

---

#### Получить все ключи

```csharp
string[] keys = API.getAllEntityData(player.handle);
```

---

### Синхронизированные данные сущности

Видны и серверу и клиентам.

#### Записать

```csharp
API.setEntitySyncedData(player.handle, "team", "Red");
```

---

#### Прочитать

```csharp
string team = API.getEntitySyncedData(player.handle, "team");
```

---

#### Проверить

```csharp
bool has = API.hasEntitySyncedData(player.handle, "team");
```

---

#### Удалить

```csharp
API.resetEntitySyncedData(player.handle, "team");
```

---

#### Получить все ключи

```csharp
string[] keys = API.getAllEntitySyncedData(player.handle);
```

---

## Сущности мира

### Создание объекта

```csharp
API.createObject(
    API.getHashKey("prop_bench_01a"),
    new Vector3(0f, 0f, 72f),
    new Vector3(0, 0, 0)
);
```

---

### Создание маркера

```csharp
API.createMarker(
    1,                              // тип маркера
    new Vector3(0f, 0f, 72f),       // позиция
    new Vector3(0, 0, 0),           // направление
    new Vector3(0, 0, 0),           // поворот
    new Vector3(1f, 1f, 1f),        // масштаб
    150,                            // прозрачность
    255, 0, 0                       // цвет (красный)
);
```

---

### Типы маркеров
```
| ID | Описание |
|---|---|
| 0 | Стрелка вниз |
| 1 | Цилиндр |
| 2 | Стрелка вверх с вращением |
| 25 | Горизонтальный круг |
| 27 | Номер |
| 28 | Стрелка (плоская) |
| 29 | Кольцо с вращением |
| 32 | Плоский круг |
```
---

### Создание текстовой метки

```csharp
API.createTextLabel(
    "~g~Добро пожаловать!",          // текст
    new Vector3(0f, 0f, 73f),        // позиция
    30f,                             // дальность видимости
    0.5f                             // размер шрифта
);
```

---

### Создание блипа на карте

```csharp
Blip blip = API.createBlip(new Vector3(0f, 0f, 72f));
API.setBlipSprite(blip.handle, 1);
API.setBlipColor(blip.handle, 1);
API.setBlipName(blip.handle, "Моя точка");
API.setBlipShortRange(blip.handle, true);
```

---

### Популярные спрайты блипов
```
| ID | Описание |
|---|---|
| 1 | Круг |
| 2 | Дом |
| 38 | Оружие |
| 56 | Гараж |
| 60 | Магазин одежды |
| 61 | Барбершоп |
| 62 | Тату-салон |
| 84 | Госпиталь |
| 88 | Полиция |
| 108 | Автосалон |
| 225 | Магазин |
| 280 | Аптека |
| 311 | Бензоколонка |
```
---

### Создание пикапа

```csharp
API.createPickup(
    PickupHash.PickupHealthStandard,
    new Vector3(0f, 0f, 72f),
    new Vector3(0, 0, 0),
    50,          // количество
    30000        // респавн через 30 сек (мс)
);
```

---

### Создание NPC (педа)

```csharp
Ped npc = API.createPed(
    PedHash.Cop01SFY,
    new Vector3(0f, 0f, 72f),
    0f    // направление взгляда
);
```

---

### Анимация для NPC

```csharp
API.playPedScenario(npc.handle, "WORLD_HUMAN_GUARD_STAND");
```

---

### Популярные сценарии NPC
```
| Сценарий | Описание |
|---|---|
| `WORLD_HUMAN_GUARD_STAND` | Стоит на охране |
| `WORLD_HUMAN_SMOKING` | Курит |
| `WORLD_HUMAN_DRINKING` | Пьёт |
| `WORLD_HUMAN_STAND_MOBILE` | Стоит с телефоном |
| `WORLD_HUMAN_CHEERING` | Ликует |
| `WORLD_HUMAN_CLIPBOARD` | Стоит с блокнотом |
| `WORLD_HUMAN_MUSICIAN` | Играет на гитаре |
| `WORLD_HUMAN_YOGA` | Занимается йогой |
| `WORLD_HUMAN_PUSH_UPS` | Отжимается |
| `WORLD_HUMAN_SIT_UPS` | Качает пресс |
```
---

## ColShape (зоны)

ColShape — это невидимые зоны на карте. Используются для определения входа/выхода игроков.

---

### Создать сферическую зону

```csharp
ColShape zone = API.createSphereColShape(
    new Vector3(0f, 0f, 72f),    // центр
    10f                           // радиус
);
```

---

### Создать цилиндрическую зону

```csharp
ColShape zone = API.createCylinderColShape(
    new Vector3(0f, 0f, 72f),    // центр
    10f,                          // радиус
    5f                            // высота
);
```

---

### Создать прямоугольную 2D зону

```csharp
ColShape zone = API.create2DColShape(
    -10f,    // X
    -10f,    // Y
    20f,     // ширина
    20f      // высота
);
```

---

### Создать прямоугольную 3D зону

```csharp
ColShape zone = API.create3DColShape(
    new Vector3(-10f, -10f, 70f),    // начало
    new Vector3(10f, 10f, 75f)       // конец
);
```

---

### Удалить зону

```csharp
API.deleteColShape(zone);
```

---

### Обработка входа и выхода

```csharp
public class ZoneResource : Script
{
    private ColShape _safeZone;

    public ZoneResource()
    {
        API.onResourceStart += OnResourceStart;
        API.onEntityEnterColShape += OnEnter;
        API.onEntityExitColShape += OnExit;
    }

    private void OnResourceStart()
    {
        _safeZone = API.createSphereColShape(new Vector3(0f, 0f, 72f), 50f);
        API.consoleOutput("Безопасная зона создана.");
    }

    private void OnEnter(ColShape shape, NetHandle entity)
    {
        if (shape != _safeZone) return;

        Client player = API.getPlayerFromHandle(entity);
        if (player == null) return;

        API.sendChatMessageToPlayer(player, "~g~Ты вошёл в безопасную зону.");
        API.setPlayerHealth(player, 100);
    }

    private void OnExit(ColShape shape, NetHandle entity)
    {
        if (shape != _safeZone) return;

        Client player = API.getPlayerFromHandle(entity);
        if (player == null) return;

        API.sendChatMessageToPlayer(player, "~r~Ты покинул безопасную зону.");
    }
}
```

---

## Удаление сущностей

### Удалить любую сущность

```csharp
API.deleteEntity(entityHandle);
```

> ⚠️ Нельзя удалить игрока этим методом. Для игроков используй `kickPlayer`.

---

### Удалить объект у конкретного игрока

```csharp
API.deleteObject(player, position, modelHash);
```

---

## Взрывы

### Создать взрыв

```csharp
API.createExplosion(ExplosionType.Grenade, new Vector3(0f, 0f, 72f), 1f);
```

---

### Создать взрыв от имени игрока

```csharp
API.createOwnedExplosion(player, ExplosionType.Grenade, new Vector3(0f, 0f, 72f), 1f);
```

---

### Типы взрывов
```
| ExplosionType | Описание |
|---|---|
| `Grenade` | Граната |
| `Molotov` | Молотов |
| `Gas` | Газ |
| `Car` | Автомобиль |
| `Plane` | Самолёт |
| `PetrolPump` | Бензоколонка |
| `Tanker` | Цистерна |
```
---

## Снаряды

### Создать снаряд

```csharp
API.createProjectile(
    WeaponHash.RPG,
    new Vector3(0f, 0f, 80f),      // откуда
    new Vector3(100f, 100f, 72f),   // куда
    100                              // урон
);
```

---

### Создать снаряд от имени игрока

```csharp
API.createOwnedProjectile(
    player,
    WeaponHash.RPG,
    player.Position,
    new Vector3(100f, 100f, 72f),
    100
);
```

---

## Эффекты частиц

### Создать эффект на позиции

```csharp
API.createParticleEffectOnPosition(
    "core",                          // библиотека
    "ent_sht_flame",                 // имя эффекта
    new Vector3(0f, 0f, 72f),        // позиция
    new Vector3(0, 0, 0),            // поворот
    1f                               // масштаб
);
```

---

### Создать зацикленный эффект

```csharp
ParticleEffect fx = API.createLoopedParticleEffectOnPosition(
    "core",
    "ent_sht_flame",
    new Vector3(0f, 0f, 72f),
    new Vector3(0, 0, 0),
    1f
);
```

---

## Информация о сервере

### Название сервера

```csharp
string name = API.getServerName();
```

---

### Установить название

```csharp
API.setServerName("My Glubinka Server");
```

---

### Порт сервера

```csharp
int port = API.getServerPort();
```

---

### Максимум игроков

```csharp
int max = API.getMaxPlayers();
```

---

### Есть ли пароль

```csharp
bool hasPassword = API.isPasswordProtected();
```

---

### Установить пароль

```csharp
API.setServerPassword("mypassword");
```

---

### Убрать пароль

```csharp
API.setServerPassword("");
```

---

### Установить имя геймода

```csharp
API.setGamemodeName("Deathmatch");
```

---

## Ресурсы

### Запустить ресурс

```csharp
API.startResource("myresource");
```

---

### Остановить ресурс

```csharp
API.stopResource("myresource");
```

---

### Проверить запущен ли ресурс

```csharp
bool running = API.isResourceRunning("myresource");
```

---

### Проверить существует ли ресурс

```csharp
bool exists = API.doesResourceExist("myresource");
```

---

### Получить список запущенных ресурсов

```csharp
string[] running = API.getRunningResources();

foreach (string r in running)
{
    API.consoleOutput("Запущен: " + r);
}
```

---

### Получить список всех ресурсов

```csharp
string[] all = API.getAllResources();

foreach (string r in all)
{
    API.consoleOutput("Ресурс: " + r);
}
```

---

## Утилиты

### Получить хэш строки

```csharp
int hash = API.getHashKey("prop_bench_01a");
```

---

### Получить SHA256 хэш

```csharp
string sha = API.getHashSHA256("mypassword");
```

---

### Случайное число

```csharp
double rnd = API.random();
```

Возвращает `double` от 0.0 до 1.0.

---

### Задержка (delay)

```csharp
// Выполнить один раз через 5 секунд
API.delay(5000, true, () =>
{
    API.sendChatMessageToAll("~y~Прошло 5 секунд!");
});
```

```csharp
// Выполнять каждые 10 секунд
API.delay(10000, false, () =>
{
    API.consoleOutput("Тик каждые 10 секунд.");
});
```

---

### Конвертация имён моделей

```csharp
VehicleHash veh = API.vehicleNameToModel("adder");
PedHash ped = API.pedNameToModel("cop01sfy");
WeaponHash wpn = API.weaponNameToModel("pistol");
PickupHash pkp = API.pickupNameToModel("pickup_health_standard");
```

---

## Частые ошибки

### Ошибка 1 — погода вне диапазона

Неправильно:
```csharp
API.setWeather(14);
```

Правильно:
```csharp
if (id >= 0 && id <= 13)
{
    API.setWeather(id);
}
```

---

### Ошибка 2 — время вне диапазона

Неправильно:
```csharp
API.setTime(25, 70);
```

Правильно:
```csharp
if (hours >= 0 && hours <= 23 && minutes >= 0 && minutes <= 59)
{
    API.setTime(hours, minutes);
}
```

---

### Ошибка 3 — забыл проверить колшейп

Неправильно:
```csharp
private void OnEnter(ColShape shape, NetHandle entity)
{
    API.sendChatMessageToPlayer(API.getPlayerFromHandle(entity), "Вошёл!");
}
```

Правильно:
```csharp
private void OnEnter(ColShape shape, NetHandle entity)
{
    if (shape != _myZone) return;

    Client player = API.getPlayerFromHandle(entity);
    if (player == null) return;

    API.sendChatMessageToPlayer(player, "Вошёл!");
}
```

---

## См. также

- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
