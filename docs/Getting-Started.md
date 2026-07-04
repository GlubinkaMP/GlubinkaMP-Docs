---
title: Getting Started
nav_order: 1
---

# Getting Started

Документация по началу работы с **Glubinka MP SDK**.

Здесь объясняется как создать первый ресурс, как работает базовый класс `GlubinkaContext`, как выводить логи и как выглядит минимальный рабочий ресурс.

---

## Что такое Glubinka MP SDK

**Glubinka MP SDK** — это публичный набор классов и инструментов для написания серверных ресурсов на платформе Glubinka MP.

SDK построен поверх ядра платформы и даёт разработчику ресурса удобный, понятный API без необходимости разбираться во внутреннем устройстве ядра.

Ты пишешь логику своего ресурса — SDK берёт на себя всё остальное.

---

## Подключение namespaces

В каждом файле ресурса нужно подключить:

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как устроен ресурс

Каждый ресурс Glubinka MP — это класс, который наследуется от `GlubinkaContext`.

`GlubinkaContext` — это базовый класс SDK. Он даёт доступ ко всем инструментам: игрокам, транспорту, миру, чату, логгеру и так далее.

### Минимальный ресурс

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Log("Мой ресурс запущен!");
    }
}
```

Это всё. Ресурс готов к работе.

---

## OnStart и OnStop

`GlubinkaContext` предоставляет два переопределяемых метода:

| Метод | Когда вызывается |
|---|---|
| `OnStart()` | Когда ресурс запускается на сервере |
| `OnStop()` | Когда ресурс останавливается |

### Пример

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Log("Ресурс запущен.");
    }

    protected override void OnStop()
    {
        Log("Ресурс остановлен.");
    }
}
```

> Оба метода необязательны. Можно переопределить только нужный.

---

## Быстрый лог из контекста

`GlubinkaContext` содержит несколько встроенных методов для быстрого вывода сообщений в консоль сервера.

Их удобно использовать прямо внутри ресурса без создания отдельного логгера.

```csharp
Log("Сообщение");              // обычное сообщение
LogError("Что-то сломалось"); // ошибка
LogWarning("Проверь это");    // предупреждение
LogSuccess("Всё прошло ок");  // успех
```

### Как выглядит вывод в консоли

```
[Glubinka] Сообщение
[Glubinka ERROR] Что-то сломалось
[Glubinka WARN] Проверь это
[Glubinka OK] Всё прошло ок
```

### Пример

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Log("Ресурс стартовал.");
        LogSuccess("Инициализация завершена.");
    }
}
```

---

## Создание Logger

Если нужен более удобный логгер с тегом и уровнями — создай его через `CreateLogger`.

```csharp
var log = CreateLogger("MyResource");
```

После этого можно использовать методы логгера:

```csharp
log.Info("Ресурс запущен.");
log.Debug("Переменная x = " + x);
log.Warning("Игрок не найден.");
log.Error("Не удалось создать транспорт.");
log.Success("Операция завершена успешно.");
```

### Как выглядит вывод в консоли

```
[MyResource] [INFO] Ресурс запущен.
[MyResource] [DEBUG] Переменная x = 42
[MyResource] [WARN] Игрок не найден.
[MyResource] [ERROR] Не удалось создать транспорт.
[MyResource] [INFO] [OK] Операция завершена успешно.
```

### Пример

```csharp
public class MyResource : GlubinkaContext
{
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("MyResource");
        _log.Info("Ресурс запущен.");
    }
}
```

> Логгер лучше хранить как поле класса, чтобы использовать его в разных методах.

---

## Форматирование сообщений логгера

Логгер поддерживает форматирование строк, как в `string.Format`.

```csharp
log.Info("Игрок {0} подключился.", playerName);
log.Debug("Позиция: {0}, {1}, {2}", x, y, z);
log.Error("Не удалось создать транспорт {0}.", modelName);
log.Warning("Игрок {0} не найден.", name);
log.Success("Ресурс {0} запущен.", resourceName);
```

---

## Работа с командами

Команды создаются через атрибут `[Command]`.

Первый параметр команды — всегда `Client client`.

Чтобы получить удобный объект `Player` из `Client`, используй метод `WrapPlayer`.

```csharp
[Command("hello")]
public void HelloCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("~g~Привет, " + player.Name + "!");
}
```

> `WrapPlayer` — это метод из `GlubinkaContext`. Он превращает внутренний `Client` в публичный `Player` со всеми удобными методами SDK.

---

## Создание транспорта

Транспорт создаётся через метод `CreateVehicle`.

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
    player.SendMessage("~g~Машина создана.");
}
```

---

## Полный пример первого ресурса

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class MyFirstResource : GlubinkaContext
{
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("MyFirstResource");
        _log.Info("Ресурс запущен и готов к работе.");
    }

    protected override void OnStop()
    {
        _log.Info("Ресурс остановлен.");
    }

    [Command("hello")]
    public void HelloCommand(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Привет, " + player.Name + "!");
        _log.Info("Игрок " + player.Name + " использовал /hello");
    }

    [Command("spawncar")]
    public void SpawnCar(Client client)
    {
        var player = WrapPlayer(client);
        var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
        player.PutIntoVehicle(car);
        player.SendMessage("~y~Машина заспавнена рядом с тобой.");
        _log.Info("Игрок " + player.Name + " заспавнил машину.");
    }

    [Command("healme")]
    public void HealMe(Client client)
    {
        var player = WrapPlayer(client);
        player.SetHealth(100);
        player.SetArmor(100);
        player.SendMessage("~g~Ты вылечен.");
    }
}
```

---

## Все методы GlubinkaContext — сводная таблица

| Метод | Описание |
|---|---|
| `OnStart()` | Вызывается при старте ресурса |
| `OnStop()` | Вызывается при остановке ресурса |
| `WrapPlayer(Client)` | Обернуть `Client` в `Player` |
| `WrapVehicle(NetHandle)` | Обернуть `NetHandle` в `Vehicle` |
| `CreateVehicle(VehicleHash, Vector3, Vector3)` | Создать транспорт |
| `CreateBlip(Vector3)` | Создать блип на карте |
| `CreateBlip(Vector3, float)` | Создать блип с радиусом |
| `CreateMarker(int, Vector3, Vector3)` | Создать 3D-маркер |
| `CreateTextLabel(string, Vector3, float)` | Создать текстовую метку |
| `CreateSphereColShape(Vector3, float)` | Создать сферический колшейп |
| `CreateCylinderColShape(Vector3, float, float)` | Создать цилиндрический колшейп |
| `CreateRectangleColShape(float, float, float, float)` | Создать прямоугольный 2D колшейп |
| `CreateBoxColShape(Vector3, Vector3)` | Создать прямоугольный 3D колшейп |
| `CreateLogger(string)` | Создать логгер с тегом |
| `Log(string)` | Быстрый лог в консоль |
| `LogError(string)` | Быстрый лог ошибки |
| `LogWarning(string)` | Быстрый лог предупреждения |
| `LogSuccess(string)` | Быстрый лог успеха |

---

## Все методы Logger — сводная таблица

| Метод | Описание |
|---|---|
| `Debug(string)` | Отладочное сообщение |
| `Info(string)` | Информационное сообщение |
| `Warning(string)` | Предупреждение |
| `Error(string)` | Ошибка |
| `Error(string, Exception)` | Ошибка с исключением |
| `Success(string)` | Успешная операция |
| `Debug(string, params object[])` | Отладка с форматированием |
| `Info(string, params object[])` | Информация с форматированием |
| `Warning(string, params object[])` | Предупреждение с форматированием |
| `Error(string, params object[])` | Ошибка с форматированием |
| `Success(string, params object[])` | Успех с форматированием |

---

## Частые ошибки

### Ошибка 1 — забыл наследоваться от GlubinkaContext

Неправильно:

```csharp
public class MyResource : Script
{
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
}
```

---

### Ошибка 2 — создал Logger без тега

Неправильно:

```csharp
var log = new Logger("", API);
```

Правильно:

```csharp
var log = CreateLogger("MyResource");
```

> Тег не может быть пустым — логгер выбросит ошибку.

---

### Ошибка 3 — работаешь с Client напрямую вместо Player

Неправильно:

```csharp
public void HelloCommand(Client client)
{
    client.sendChatMessage("Привет!");
}
```

Правильно:

```csharp
public void HelloCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SendMessage("Привет!");
}
```

---

### Ошибка 4 — создаёшь Logger каждый раз заново

Неправильно:

```csharp
[Command("hello")]
public void HelloCommand(Client client)
{
    var log = CreateLogger("MyResource");
    log.Info("...");
}
```

Правильно:

```csharp
private Logger _log;

protected override void OnStart()
{
    _log = CreateLogger("MyResource");
}

[Command("hello")]
public void HelloCommand(Client client)
{
    _log.Info("...");
}
```

---

## См. также
