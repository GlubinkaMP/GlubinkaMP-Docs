---
title: Blip
nav_order: 7
---

# Blip

Документация по работе с блипами в **Glubinka MP SDK**.

Блип — это метка на карте и миникарте. Используется для обозначения магазинов, зон, точек интереса и любых других мест на сервере.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как создать Blip

Блип создаётся через методы `CreateBlip` из `GlubinkaContext`.

### Простой блип по позиции

```csharp
var blip = CreateBlip(new Vector3(0, 0, 72));
```

### Блип с радиусом (зона на карте)

```csharp
var blip = CreateBlip(new Vector3(0, 0, 72), 50f);
```

### Пример

```csharp
protected override void OnStart()
{
    var blip = CreateBlip(new Vector3(-269.0f, -955.0f, 31.0f));
    blip.SetName("Центр города");
    blip.SetColor(2);
    blip.SetSprite(1);
}
```

---

## Основные свойства

```csharp
NetHandle handle  = blip.Handle;
bool exists       = blip.Exists;
string name       = blip.Name;
int color         = blip.Color;
int sprite        = blip.Sprite;
float scale       = blip.Scale;
int transparency  = blip.Transparency;
bool shortRange   = blip.ShortRange;
Vector3 position  = blip.Position;
bool routeVisible = blip.RouteVisible;
int routeColor    = blip.RouteColor;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Handle` | `NetHandle` | Внутренний handle блипа |
| `Exists` | `bool` | Существует ли блип |
| `Name` | `string` | Название на карте (чтение / запись) |
| `Color` | `int` | Цвет блипа (чтение / запись) |
| `Sprite` | `int` | Иконка блипа (чтение / запись) |
| `Scale` | `float` | Масштаб блипа (чтение / запись) |
| `Transparency` | `int` | Прозрачность 0–255 (чтение / запись) |
| `ShortRange` | `bool` | Виден только поблизости (чтение / запись) |
| `Position` | `Vector3` | Позиция блипа (чтение / запись) |
| `RouteVisible` | `bool` | Виден ли маршрут (чтение / запись) |
| `RouteColor` | `int` | Цвет маршрута (чтение / запись) |

---

## Имя блипа

```csharp
blip.Name = "Магазин";
blip.SetName("Магазин");   // alias
```

### Пример

```csharp
var blip = CreateBlip(new Vector3(25.0f, -1345.0f, 29.0f));
blip.SetName("Оружейный магазин");
```

---

## Цвет блипа

```csharp
blip.Color = 1;
blip.SetColor(1);   // alias
```

### Популярные цвета

| Число | Цвет |
|---|---|
| `0` | Белый |
| `1` | Красный |
| `2` | Зелёный |
| `3` | Синий |
| `4` | Белый (2) |
| `5` | Жёлтый |
| `17` | Оранжевый |
| `40` | Розовый |
| `42` | Голубой |
| `53` | Фиолетовый |

### Пример

```csharp
var blip = CreateBlip(new Vector3(440.0f, -981.0f, 30.0f));
blip.SetName("Больница");
blip.SetColor(2);   // зелёный
```

---

## Иконка блипа

```csharp
blip.Sprite = 1;
blip.SetSprite(1);   // alias
```

### Популярные иконки

| Число | Иконка |
|---|---|
| `1` | Белый кружок |
| `2` | Машина |
| `3` | Склад |
| `4` | Магазин |
| `60` | Больница |
| `68` | Оружие |
| `110` | Звезда (VIP) |
| `153` | Гараж |
| `351` | Дом |
| `408` | Банк |

### Пример

```csharp
var blip = CreateBlip(new Vector3(149.0f, -1040.0f, 29.0f));
blip.SetName("Полиция");
blip.SetSprite(60);
blip.SetColor(3);   // синий
```

---

## Масштаб блипа

```csharp
blip.Scale = 1.5f;
blip.SetScale(1.5f);   // alias
```

> Значение по умолчанию — `1.0`. Меньше — блип меньше, больше — крупнее.

### Пример

```csharp
var blip = CreateBlip(new Vector3(0, 0, 72));
blip.SetName("Важная зона");
blip.SetScale(2.0f);   // увеличенный блип
```

---

## Прозрачность блипа

```csharp
blip.Transparency = 128;
blip.SetTransparency(128);   // alias
```

> Допустимые значения: от `0` (полностью прозрачный) до `255` (полностью виден).

---

## Позиция блипа

### Переместить блип

```csharp
blip.Position = new Vector3(100, 200, 30);
blip.MoveTo(new Vector3(100, 200, 30));
blip.MoveTo(100f, 200f, 30f);
blip.SetPosition(new Vector3(100, 200, 30));    // alias
blip.SetPosition(100f, 200f, 30f);              // alias
```

| Метод | Описание |
|---|---|
| `MoveTo(Vector3)` | Переместить блип |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |

---

## Short Range

Если включён `ShortRange` — блип виден на карте только когда игрок находится поблизости. Удобно для мелких точек, чтобы не засорять карту.

```csharp
blip.ShortRange = true;
blip.SetShortRange(true);   // alias
blip.EnableShortRange();    // alias включить
blip.DisableShortRange();   // alias выключить
```

| Метод | Описание |
|---|---|
| `SetShortRange(bool)` | Включить / выключить |
| `EnableShortRange()` | Включить |
| `DisableShortRange()` | Выключить |

### Пример

```csharp
var blip = CreateBlip(new Vector3(440.0f, -981.0f, 30.0f));
blip.SetName("Аптека");
blip.SetSprite(4);
blip.EnableShortRange();   // виден только поблизости
```

---

## Маршрут до блипа

Маршрут — это линия на миникарте, которая ведёт игрока к блипу.

### Показать маршрут

```csharp
blip.RouteVisible = true;
blip.SetRouteVisible(true);   // alias
blip.EnableRoute();           // alias
blip.EnableRoute(3);          // включить и сразу задать цвет
blip.ShowRoute();             // alias
blip.ShowRoute(3);            // alias с цветом
```

### Скрыть маршрут

```csharp
blip.RouteVisible = false;
blip.SetRouteVisible(false);  // alias
blip.DisableRoute();          // alias
blip.HideRoute();             // alias
```

### Установить цвет маршрута

```csharp
blip.RouteColor = 3;
blip.SetRouteColor(3);   // alias
```

### Установить цвет и видимость сразу

```csharp
blip.SetRoute(3, true);   // цвет 3, маршрут виден
blip.SetRoute(3, false);  // цвет 3, маршрут скрыт
```

| Метод | Описание |
|---|---|
| `EnableRoute()` | Показать маршрут |
| `EnableRoute(int)` | Показать маршрут с цветом |
| `DisableRoute()` | Скрыть маршрут |
| `ShowRoute(int)` | Alias для EnableRoute |
| `HideRoute()` | Alias для DisableRoute |
| `SetRouteVisible(bool)` | Показать / скрыть маршрут |
| `SetRouteColor(int)` | Установить цвет маршрута |
| `SetRoute(int, bool)` | Цвет и видимость сразу |

### Пример

```csharp
[Command("route")]
public void Route(Client client)
{
    var player = WrapPlayer(client);
    var blip = CreateBlip(new Vector3(-1034.0f, -2733.0f, 20.0f));
    blip.SetName("Аэропорт");
    blip.SetColor(3);
    blip.EnableRoute(3);
    player.SendSuccess("Маршрут до аэропорта построен.");
}
```

---

## Удалить блип

```csharp
blip.Delete();
```

> Перед удалением автоматически проверяется существование блипа.

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class BlipDemo : GlubinkaContext
{
    private Blip _shopBlip;
    private Blip _hospitalBlip;
    private Blip _policeBlip;

    protected override void OnStart()
    {
        // Оружейный магазин
        _shopBlip = CreateBlip(new Vector3(25.0f, -1345.0f, 29.0f));
        _shopBlip.SetName("Оружейный магазин");
        _shopBlip.SetSprite(68);
        _shopBlip.SetColor(1);
        _shopBlip.EnableShortRange();

        // Больница
        _hospitalBlip = CreateBlip(new Vector3(295.0f, -584.0f, 43.0f));
        _hospitalBlip.SetName("Больница");
        _hospitalBlip.SetSprite(60);
        _hospitalBlip.SetColor(2);
        _hospitalBlip.EnableShortRange();

        // Полиция
        _policeBlip = CreateBlip(new Vector3(441.0f, -981.0f, 30.0f));
        _policeBlip.SetName("Полицейский участок");
        _policeBlip.SetSprite(60);
        _policeBlip.SetColor(3);
        _policeBlip.EnableShortRange();

        Log("Блипы созданы.");
    }

    [Command("hospital")]
    public void Hospital(Client client)
    {
        var player = WrapPlayer(client);
        _hospitalBlip.EnableRoute(2);
        player.SendSuccess("Маршрут до больницы построен.");
    }

    [Command("clearroute")]
    public void ClearRoute(Client client)
    {
        var player = WrapPlayer(client);
        _hospitalBlip.DisableRoute();
        _shopBlip.DisableRoute();
        _policeBlip.DisableRoute();
        player.SendMessage("~y~Маршруты сброшены.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — создал блип вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void Init()
    {
        var blip = CreateBlip(new Vector3(0, 0, 0));
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        var blip = CreateBlip(new Vector3(0, 0, 0));
    }
}
```

---

### Ошибка 2 — передал отрицательный Scale

Неправильно:

```csharp
blip.SetScale(-1f);
```

Правильно:

```csharp
blip.SetScale(1.5f);
```

> Scale не может быть меньше `0` — SDK выбросит ошибку.

---

### Ошибка 3 — передал Transparency вне диапазона

Неправильно:

```csharp
blip.SetTransparency(300);
```

Правильно:

```csharp
blip.SetTransparency(200);  // допустимо от 0 до 255
```

---

### Ошибка 4 — работаешь с блипом после Delete

Неправильно:

```csharp
blip.Delete();
blip.SetName("Что-то");
```

Правильно:

```csharp
if (blip.Exists)
{
    blip.SetName("Что-то");
}
```

---

### Ошибка 5 — хранишь блип как локальную переменную и теряешь его

Неправильно:

```csharp
protected override void OnStart()
{
    var blip = CreateBlip(new Vector3(0, 0, 72));
    blip.SetName("Точка");
}
// blip недоступен нигде кроме OnStart
```

Правильно — храни как поле класса:

```csharp
private Blip _myBlip;

protected override void OnStart()
{
    _myBlip = CreateBlip(new Vector3(0, 0, 72));
    _myBlip.SetName("Точка");
}
```

---

## Все методы Blip — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Handle` | Внутренний handle |
| `Exists` | Существует ли блип |
| `Name` | Название (чтение / запись) |
| `Color` | Цвет (чтение / запись) |
| `Sprite` | Иконка (чтение / запись) |
| `Scale` | Масштаб (чтение / запись) |
| `Transparency` | Прозрачность 0–255 (чтение / запись) |
| `ShortRange` | Виден только поблизости (чтение / запись) |
| `Position` | Позиция (чтение / запись) |
| `RouteVisible` | Виден ли маршрут (чтение / запись) |
| `RouteColor` | Цвет маршрута (чтение / запись) |
| `Delete()` | Удалить блип |
| `MoveTo(Vector3)` | Переместить блип |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |
| `SetName(string)` | Установить имя |
| `SetColor(int)` | Установить цвет |
| `SetSprite(int)` | Установить иконку |
| `SetScale(float)` | Установить масштаб |
| `SetTransparency(int)` | Установить прозрачность |
| `SetShortRange(bool)` | Включить / выключить short range |
| `EnableShortRange()` | Включить short range |
| `DisableShortRange()` | Выключить short range |
| `SetRouteVisible(bool)` | Показать / скрыть маршрут |
| `SetRouteColor(int)` | Установить цвет маршрута |
| `SetRoute(int, bool)` | Цвет и видимость маршрута |
| `EnableRoute()` | Показать маршрут |
| `EnableRoute(int)` | Показать маршрут с цветом |
| `DisableRoute()` | Скрыть маршрут |
| `ShowRoute(int)` | Alias для EnableRoute |
| `HideRoute()` | Alias для DisableRoute |

---

## См. также

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicle.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html)
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
