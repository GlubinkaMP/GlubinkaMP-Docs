# Logger

Документация по логированию в **Glubinka MP SDK**.

`Logger` — это класс для вывода сообщений в консоль сервера с тегом и уровнем. Удобен для отладки, отслеживания ошибок и контроля работы ресурса.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
```

---

## Как создать Logger

Логгер создаётся через метод `CreateLogger` из `GlubinkaContext`.

```csharp
var log = CreateLogger("MyResource");
```

Или напрямую через конструктор — но внутри `GlubinkaContext` удобнее через `CreateLogger`.

```csharp
var log = new Logger("MyResource", API);
```

> Тег не может быть пустым — логгер выбросит ошибку.

### Рекомендация

Храни логгер как поле класса, чтобы использовать его во всех методах ресурса.

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

---

## Уровни логирования

В SDK есть пять уровней:

| Метод | Уровень | Префикс в консоли | Описание |
|---|---|---|---|
| `Debug` | `LogLevel.Debug` | `[DEBUG]` | Отладочная информация |
| `Info` | `LogLevel.Info` | `[INFO]` | Обычное сообщение |
| `Warning` | `LogLevel.Warning` | `[WARN]` | Предупреждение |
| `Error` | `LogLevel.Error` | `[ERROR]` | Ошибка |
| `Success` | `LogLevel.Info` | `[INFO] [OK]` | Успешная операция |

---

## Основные методы

### Debug

```csharp
log.Debug("Переменная x = " + x);
```

### Info

```csharp
log.Info("Ресурс запущен.");
```

### Warning

```csharp
log.Warning("Игрок не найден.");
```

### Error

```csharp
log.Error("Не удалось создать транспорт.");
```

### Error с исключением

```csharp
try
{
    // какой-то код
}
catch (Exception ex)
{
    log.Error("Произошла ошибка", ex);
}
```

### Success

```csharp
log.Success("Игрок успешно создан.");
```

---

## Как выглядит вывод в консоли

```
[MyResource] [DEBUG] Переменная x = 42
[MyResource] [INFO] Ресурс запущен.
[MyResource] [WARN] Игрок не найден.
[MyResource] [ERROR] Не удалось создать транспорт.
[MyResource] [ERROR] Произошла ошибка | Object reference not set
[MyResource] [INFO] [OK] Игрок успешно создан.
```

Формат строки:
```
[тег] [уровень] сообщение
```

---

## Форматирование сообщений

Все методы поддерживают форматирование строк — как в `string.Format`.

```csharp
log.Debug("Позиция: {0}, {1}, {2}", x, y, z);
log.Info("Игрок {0} подключился.", playerName);
log.Warning("Игрок {0} не найден.", name);
log.Error("Не удалось создать транспорт {0}.", modelName);
log.Success("Ресурс {0} запущен.", resourceName);
```

### Пример с несколькими аргументами

```csharp
log.Info("Игрок {0} телепортирован в {1}, {2}, {3}.", player.Name, x, y, z);
log.Warning("Попытка {0} из {1} не удалась.", attempt, maxAttempts);
```

---

## Быстрые методы из GlubinkaContext

Если не нужен полноценный логгер — в `GlubinkaContext` есть встроенные быстрые методы.

```csharp
Log("Сообщение");              // [Glubinka] Сообщение
LogError("Что-то сломалось"); // [Glubinka ERROR] Что-то сломалось
LogWarning("Проверь это");    // [Glubinka WARN] Проверь это
LogSuccess("Всё ок");         // [Glubinka OK] Всё ок
```

### Разница между Logger и быстрыми методами

| | `Logger` | `Log / LogError / ...` |
|---|---|---|
| Тег | Свой (`MyResource`) | Всегда `Glubinka` |
| Уровни | Debug, Info, Warning, Error, Success | Info, Error, Warning, Success |
| Форматирование | ✅ Есть | ❌ Нет |
| Удобство | Для больших ресурсов | Для быстрых сообщений |

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class LoggerDemo : GlubinkaContext
{
    private Logger _log;

    protected override void OnStart()
    {
        _log = CreateLogger("LoggerDemo");

        _log.Info("Ресурс запущен.");
        _log.Success("Инициализация завершена.");
        _log.Debug("Режим отладки активен.");
    }

    protected override void OnStop()
    {
        _log.Info("Ресурс остановлен.");
    }

    [Command("heal")]
    public void Heal(Client client)
    {
        var player = WrapPlayer(client);

        _log.Debug("Команда /heal от игрока {0}", player.Name);

        player.SetHealth(100);
        player.SetArmor(100);
        player.SendSuccess("Ты вылечен.");

        _log.Info("Игрок {0} вылечен.", player.Name);
    }

    [Command("spawncar")]
    public void SpawnCar(Client client, string modelName)
    {
        var player = WrapPlayer(client);

        _log.Debug("Игрок {0} запросил машину {1}", player.Name, modelName);

        try
        {
            var car = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
            player.PutIntoVehicle(car);
            player.SendSuccess("Машина создана.");
            _log.Success("Машина для {0} создана.", player.Name);
        }
        catch (Exception ex)
        {
            _log.Error("Не удалось создать машину для {0}", ex, player.Name);
            player.SendError("Ошибка при создании машины.");
        }
    }

    [Command("kick")]
    public void Kick(Client client, string targetName, string reason)
    {
        var player = WrapPlayer(client);

        if (!Players.TryGetByName(targetName, out Player target))
        {
            player.SendError("Игрок не найден.");
            _log.Warning("Игрок {0} не найден для кика.", targetName);
            return;
        }

        target.Kick(reason);
        _log.Info("Игрок {0} кикнут администратором {1}. Причина: {2}",
            target.Name, player.Name, reason);
    }
}
```

---

## Частые ошибки

### Ошибка 1 — создал Logger с пустым тегом

Неправильно:

```csharp
var log = new Logger("", API);
```

Правильно:

```csharp
var log = CreateLogger("MyResource");
```

---

### Ошибка 2 — создаёт Logger каждый раз заново

Неправильно:

```csharp
[Command("heal")]
public void Heal(Client client)
{
    var log = CreateLogger("MyResource");
    log.Info("...");
}
```

Правильно — один раз в `OnStart`, храни как поле:

```csharp
private Logger _log;

protected override void OnStart()
{
    _log = CreateLogger("MyResource");
}

[Command("heal")]
public void Heal(Client client)
{
    _log.Info("...");
}
```

---

### Ошибка 3 — использует Logger до OnStart

Неправильно:

```csharp
private Logger _log = CreateLogger("MyResource");   // ошибка — API ещё не готов
```

Правильно:

```csharp
private Logger _log;

protected override void OnStart()
{
    _log = CreateLogger("MyResource");
}
```

---

### Ошибка 4 — передал null в Error с исключением

Неправильно:

```csharp
log.Error("Ошибка", null);
```

Правильно:

```csharp
try
{
    // код
}
catch (Exception ex)
{
    log.Error("Ошибка", ex);
}
```

---

### Ошибка 5 — отправил пустое сообщение

Пустые сообщения игнорируются автоматически — логгер проверяет строку на пустоту.

Но лучше не передавать пустые строки намеренно:

```csharp
// это ничего не выведет
log.Info("");
log.Debug(null);
```

---

## Все методы Logger — сводная таблица

| Метод | Описание |
|---|---|
| `Debug(string)` | Отладочное сообщение |
| `Debug(string, params object[])` | Отладка с форматированием |
| `Info(string)` | Информационное сообщение |
| `Info(string, params object[])` | Информация с форматированием |
| `Warning(string)` | Предупреждение |
| `Warning(string, params object[])` | Предупреждение с форматированием |
| `Error(string)` | Ошибка |
| `Error(string, params object[])` | Ошибка с форматированием |
| `Error(string, Exception)` | Ошибка с исключением |
| `Success(string)` | Успешная операция |
| `Success(string, params object[])` | Успех с форматированием |

## Enum LogLevel — сводная таблица

| Значение | Описание |
|---|---|
| `LogLevel.Debug` | Отладочная информация |
| `LogLevel.Info` | Обычная информация |
| `LogLevel.Warning` | Предупреждение |
| `LogLevel.Error` | Ошибка |

---

## См. также
