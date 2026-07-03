# ColShape

Документация по работе с колшейпами в **Glubinka MP SDK**.

ColShape — это невидимая зона в игровом мире. Когда игрок или транспорт входит в зону или выходит из неё — срабатывают серверные события. Используется для точек входа, магазинов, работ, триггеров и любых других интерактивных зон.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Типы колшейпов

В SDK есть четыре типа колшейпов.

| Тип (`ColShapeType`) | Описание |
|---|---|
| `ColShapeType.Sphere` | Сфера — круглая зона с радиусом |
| `ColShapeType.Cylinder` | Цилиндр — круглая зона с радиусом и высотой |
| `ColShapeType.Rectangle2D` | Плоский прямоугольник (2D) |
| `ColShapeType.Rectangle3D` | Объёмный прямоугольник (3D) |

---

## Как создать ColShape

Все колшейпы создаются через методы `GlubinkaContext`.

### Сфера

```csharp
var zone = CreateSphereColShape(new Vector3(0, 0, 72), 5f);
```

- позиция центра
- радиус сферы

### Цилиндр

```csharp
var zone = CreateCylinderColShape(new Vector3(0, 0, 72), 5f, 10f);
```

- позиция центра
- радиус
- высота

### Прямоугольник 2D

```csharp
var zone = CreateRectangleColShape(0f, 0f, 100f, 100f);
```

- X, Y — начальная точка
- ширина, высота

### Прямоугольник 3D

```csharp
var zone = CreateBoxColShape(new Vector3(0, 0, 72), new Vector3(10, 10, 5));
```

- позиция
- размер (X, Y, Z)

| Метод | Тип | Описание |
|---|---|---|
| `CreateSphereColShape(Vector3, float)` | Sphere | Создать сферу |
| `CreateCylinderColShape(Vector3, float, float)` | Cylinder | Создать цилиндр |
| `CreateRectangleColShape(float, float, float, float)` | Rectangle2D | Создать плоский прямоугольник |
| `CreateBoxColShape(Vector3, Vector3)` | Rectangle3D | Создать объёмный прямоугольник |

---

## Основные свойства

```csharp
ColShapeType type  = zone.Type;
bool isSphere      = zone.IsSphere;
bool isCylinder    = zone.IsCylinder;
bool isRect2D      = zone.IsRectangle2D;
bool isRect3D      = zone.IsRectangle3D;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Type` | `ColShapeType` | Тип колшейпа |
| `InternalShape` | `ColShape` | Внутренний объект (нужен для событий) |
| `IsSphere` | `bool` | Является ли сферой |
| `IsCylinder` | `bool` | Является ли цилиндром |
| `IsRectangle2D` | `bool` | Является ли 2D прямоугольником |
| `IsRectangle3D` | `bool` | Является ли 3D прямоугольником |

---

## Удалить колшейп

```csharp
zone.Delete();
```

---

## События входа и выхода

Колшейп сам по себе ничего не делает — он только отслеживает, когда кто-то входит или выходит.

Логика срабатывает через серверные события:

- `onEntityEnterColShape` — кто-то вошёл в зону
- `onEntityExitColShape` — кто-то вышел из зоны

### Как подписаться на событие

```csharp
API.onEntityEnterColShape += OnEnter;
API.onEntityExitColShape  += OnExit;
```

### Как проверить, что сработал нужный колшейп

Используй метод `IsSame` — он сравнивает внутренний shape события с твоим колшейпом.

```csharp
private void OnEnter(ColShape shape, NetHandle entity)
{
    if (_myZone.IsSame(shape))
    {
        // игрок вошёл в нашу зону
    }
}
```

> `ColShape` в параметре события — это внутренний тип ядра `GTANetworkServer.ColShape`, не SDK-класс. Метод `IsSame` берёт на себя это сравнение.

---

## Полный пример — зона входа

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class ColShapeDemo : GlubinkaContext
{
    private ColShape _shopZone;
    private ColShape _hospitalZone;

    protected override void OnStart()
    {
        // Зона магазина — сфера радиусом 3 метра
        _shopZone = CreateSphereColShape(
            new Vector3(25.0f, -1345.0f, 29.0f),
            3f
        );

        // Зона больницы — цилиндр радиусом 5 метров, высотой 3 метра
        _hospitalZone = CreateCylinderColShape(
            new Vector3(295.0f, -584.0f, 43.0f),
            5f,
            3f
        );

        API.onEntityEnterColShape += OnEnterShape;
        API.onEntityExitColShape  += OnExitShape;

        Log("Зоны созданы.");
    }

    private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
    {
        // Проверяем, что entity — это игрок
        if (!API.isPlayer(entity))
            return;

        var client = API.getPlayerFromHandle(entity);
        if (client == null)
            return;

        var player = WrapPlayer(client);

        if (_shopZone.IsSame(shape))
        {
            player.SendMessage("~g~Ты вошёл в магазин. Нажми ~y~E ~g~для покупки.");
        }
        else if (_hospitalZone.IsSame(shape))
        {
            player.SendMessage("~g~Ты вошёл в больницу.");
            player.SetHealth(100);
            player.SetArmor(100);
        }
    }

    private void OnExitShape(GTANetworkServer.ColShape shape, NetHandle entity)
    {
        if (!API.isPlayer(entity))
            return;

        var client = API.getPlayerFromHandle(entity);
        if (client == null)
            return;

        var player = WrapPlayer(client);

        if (_shopZone.IsSame(shape))
        {
            player.SendMessage("~y~Ты вышел из магазина.");
        }
        else if (_hospitalZone.IsSame(shape))
        {
            player.SendMessage("~y~Ты вышел из больницы.");
        }
    }

    protected override void OnStop()
    {
        API.onEntityEnterColShape -= OnEnterShape;
        API.onEntityExitColShape  -= OnExitShape;

        _shopZone.Delete();
        _hospitalZone.Delete();
    }
}
```

---

## Пример — зона с транспортом

```csharp
private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    if (_myZone.IsSame(shape))
    {
        // Проверяем тип — игрок или транспорт
        if (API.isPlayer(entity))
        {
            var client = API.getPlayerFromHandle(entity);
            var player = WrapPlayer(client);
            player.SendMessage("~g~Ты вошёл в зону.");
        }
        else if (API.isVehicle(entity))
        {
            var vehicle = WrapVehicle(entity);

            if (vehicle.TryGetDriver(out Player driver))
            {
                driver.SendMessage("~y~Твоя машина въехала в зону.");
            }
        }
    }
}
```

---

## Пример — несколько зон с разными типами

```csharp
protected override void OnStart()
{
    // Сфера — точка спавна
    var spawnZone = CreateSphereColShape(
        new Vector3(-269.0f, -955.0f, 31.0f),
        3f
    );

    // Цилиндр — рабочая зона
    var jobZone = CreateCylinderColShape(
        new Vector3(120.0f, -620.0f, 43.0f),
        4f,
        3f
    );

    // 2D прямоугольник — парковка
    var parkingZone = CreateRectangleColShape(
        -270f, -960f, 30f, 20f
    );

    // 3D прямоугольник — склад
    var warehouseZone = CreateBoxColShape(
        new Vector3(500f, -1000f, 25f),
        new Vector3(20f, 20f, 5f)
    );

    API.onEntityEnterColShape += OnEnterShape;
}
```

---

## Проверка типа колшейпа

Если у тебя несколько колшейпов разных типов — можно проверять тип для дополнительной логики.

```csharp
if (zone.IsSphere)
{
    Log("Это сфера.");
}

if (zone.IsCylinder)
{
    Log("Это цилиндр.");
}

// или через enum
if (zone.Type == ColShapeType.Rectangle3D)
{
    Log("Это 3D прямоугольник.");
}
```

---

## Частые ошибки

### Ошибка 1 — создал ColShape вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void Init()
    {
        var zone = CreateSphereColShape(new Vector3(0, 0, 0), 5f);
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        var zone = CreateSphereColShape(new Vector3(0, 0, 0), 5f);
    }
}
```

---

### Ошибка 2 — не проверил IsSame и реагируешь на все зоны сразу

Неправильно:

```csharp
private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    var client = API.getPlayerFromHandle(entity);
    var player = WrapPlayer(client);
    player.SendMessage("Ты вошёл в зону.");  // сработает для ВСЕХ колшейпов на сервере
}
```

Правильно:

```csharp
private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    if (!_myZone.IsSame(shape))
        return;

    var client = API.getPlayerFromHandle(entity);
    var player = WrapPlayer(client);
    player.SendMessage("Ты вошёл в нашу зону.");
}
```

---

### Ошибка 3 — не проверил, что entity это игрок

Неправильно:

```csharp
private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    var client = API.getPlayerFromHandle(entity);
    var player = WrapPlayer(client);   // упадёт если entity — транспорт
    player.SendMessage("Привет!");
}
```

Правильно:

```csharp
private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    if (!API.isPlayer(entity))
        return;

    var client = API.getPlayerFromHandle(entity);
    if (client == null)
        return;

    var player = WrapPlayer(client);
    player.SendMessage("Привет!");
}
```

---

### Ошибка 4 — забыл отписаться от события при остановке ресурса

Неправильно:

```csharp
protected override void OnStart()
{
    API.onEntityEnterColShape += OnEnterShape;
}

// OnStop не переопределён — подписка висит вечно
```

Правильно:

```csharp
protected override void OnStart()
{
    API.onEntityEnterColShape += OnEnterShape;
}

protected override void OnStop()
{
    API.onEntityEnterColShape -= OnEnterShape;
}
```

---

### Ошибка 5 — хранишь ColShape как локальную переменную

Неправильно:

```csharp
protected override void OnStart()
{
    var zone = CreateSphereColShape(new Vector3(0, 0, 72), 5f);
}
// zone недоступен в событиях
```

Правильно — храни как поле класса:

```csharp
private ColShape _zone;

protected override void OnStart()
{
    _zone = CreateSphereColShape(new Vector3(0, 0, 72), 5f);
    API.onEntityEnterColShape += OnEnterShape;
}

private void OnEnterShape(GTANetworkServer.ColShape shape, NetHandle entity)
{
    if (_zone.IsSame(shape))
    {
        // логика
    }
}
```

---

## Все методы ColShape — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Type` | Тип колшейпа |
| `InternalShape` | Внутренний объект ядра |
| `IsSphere` | Является ли сферой |
| `IsCylinder` | Является ли цилиндром |
| `IsRectangle2D` | Является ли 2D прямоугольником |
| `IsRectangle3D` | Является ли 3D прямоугольником |
| `IsSame(ColShape)` | Совпадает ли с колшейпом из события |
| `Delete()` | Удалить колшейп |

## Методы создания ColShape — сводная таблица

| Метод в GlubinkaContext | Описание |
|---|---|
| `CreateSphereColShape(Vector3, float)` | Создать сферу |
| `CreateCylinderColShape(Vector3, float, float)` | Создать цилиндр |
| `CreateRectangleColShape(float, float, float, float)` | Создать 2D прямоугольник |
| `CreateBoxColShape(Vector3, Vector3)` | Создать 3D прямоугольник |

---

## См. также
