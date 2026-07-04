---
title: Marker
nav_order: 8
---

# Marker

Документация по работе с 3D-маркерами в **Glubinka MP SDK**.

Маркер — это объёмная фигура в игровом мире: стрелка, кольцо, цилиндр и т.п. Используется для обозначения точек входа, зон взаимодействия, работ, магазинов и других мест.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как создать Marker

Маркер создаётся через метод `CreateMarker` из `GlubinkaContext`.

```csharp
var marker = CreateMarker(type, position, scale);
```

- `type` — тип маркера (число)
- `position` — позиция в мире
- `scale` — размер по осям X, Y, Z

### Пример

```csharp
protected override void OnStart()
{
    var marker = CreateMarker(1, new Vector3(-269.0f, -955.0f, 31.0f), new Vector3(2, 2, 2));
    marker.SetColor(0, 255, 0);   // зелёный
}
```

---

## Типы маркеров

| Число | Описание |
|---|---|
| `0` | Стрелка вниз |
| `1` | Кольцо (цилиндр) |
| `2` | Кольцо горизонтальное |
| `3` | Квадрат горизонтальный |
| `4` | Квадрат вертикальный |
| `6` | Шар |
| `7` | Цилиндр вверх |
| `20` | Стрелка вверх |
| `25` | Ромб вертикальный |
| `28` | Кольцо тонкое |

> Самые распространённые: `0` — стрелка вниз, `1` — кольцо, `6` — шар.

---

## Основные свойства

```csharp
NetHandle handle  = marker.Handle;
bool exists       = marker.Exists;
Vector3 position  = marker.Position;
Vector3 rotation  = marker.Rotation;
Vector3 scale     = marker.Scale;
Vector3 direction = marker.Direction;
int type          = marker.Type;
Color color       = marker.Color;
bool bobbing      = marker.BobUpAndDown;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Handle` | `NetHandle` | Внутренний handle маркера |
| `Exists` | `bool` | Существует ли маркер |
| `Position` | `Vector3` | Позиция в мире (чтение / запись) |
| `Rotation` | `Vector3` | Поворот (чтение / запись) |
| `Scale` | `Vector3` | Масштаб по осям (чтение / запись) |
| `Direction` | `Vector3` | Направление (чтение / запись) |
| `Type` | `int` | Тип маркера (чтение / запись) |
| `Color` | `Color` | Цвет маркера (чтение / запись) |
| `BobUpAndDown` | `bool` | Плавание вверх-вниз (чтение / запись) |

---

## Позиция маркера

```csharp
marker.Position = new Vector3(100, 200, 30);
marker.MoveTo(new Vector3(100, 200, 30));
marker.MoveTo(100f, 200f, 30f);
marker.SetPosition(new Vector3(100, 200, 30));   // alias
marker.SetPosition(100f, 200f, 30f);             // alias
```

| Метод | Описание |
|---|---|
| `MoveTo(Vector3)` | Переместить маркер |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |

---

## Поворот маркера

```csharp
marker.Rotation = new Vector3(0, 0, 90);
marker.RotateTo(new Vector3(0, 0, 90));
marker.RotateTo(0f, 0f, 90f);
marker.SetRotation(new Vector3(0, 0, 90));   // alias
marker.SetRotation(0f, 0f, 90f);             // alias
```

| Метод | Описание |
|---|---|
| `RotateTo(Vector3)` | Повернуть маркер |
| `RotateTo(float, float, float)` | Повернуть по координатам |
| `SetRotation(Vector3)` | Alias для RotateTo |
| `SetRotation(float, float, float)` | Alias для RotateTo |

---

## Тип маркера

```csharp
marker.Type = 1;
marker.SetType(1);   // alias
```

---

## Масштаб маркера

### По осям отдельно

```csharp
marker.Scale = new Vector3(2, 2, 2);
marker.SetScale(new Vector3(2, 2, 2));
marker.SetScale(2f, 2f, 2f);
```

### Одинаковый масштаб по всем осям

```csharp
marker.SetUniformScale(2f);   // устанавливает Vector3(2, 2, 2)
```

| Метод | Описание |
|---|---|
| `SetScale(Vector3)` | Масштаб по осям |
| `SetScale(float, float, float)` | Масштаб по координатам |
| `SetUniformScale(float)` | Одинаковый масштаб по всем осям |

### Пример

```csharp
var marker = CreateMarker(1, new Vector3(0, 0, 72), new Vector3(1, 1, 1));
marker.SetUniformScale(3f);   // большое кольцо
```

---

## Цвет маркера

### Через объект Color

```csharp
marker.Color = new Color(255, 0, 0, 255);
marker.SetColor(new Color(255, 0, 0, 255));
```

### Через ARGB значения

```csharp
marker.SetColor(255, 255, 0, 0);   // alpha, red, green, blue
```

### Без прозрачности (alpha = 255)

```csharp
marker.SetColor(255, 0, 0);   // red, green, blue — alpha будет 255
```

| Метод | Описание |
|---|---|
| `SetColor(Color)` | Цвет через объект Color |
| `SetColor(int, int, int, int)` | Цвет через ARGB |
| `SetColor(int, int, int)` | Цвет через RGB, alpha = 255 |

### Популярные цвета

| Вызов | Цвет |
|---|---|
| `SetColor(255, 0, 0)` | Красный |
| `SetColor(0, 255, 0)` | Зелёный |
| `SetColor(0, 0, 255)` | Синий |
| `SetColor(255, 255, 0)` | Жёлтый |
| `SetColor(255, 165, 0)` | Оранжевый |
| `SetColor(255, 255, 255)` | Белый |
| `SetColor(128, 0, 255, 100)` | Полупрозрачный фиолетовый |

### Пример

```csharp
var marker = CreateMarker(1, new Vector3(0, 0, 72), new Vector3(2, 2, 2));
marker.SetColor(155, 0, 255, 0);   // полупрозрачный зелёный
```

---

## Направление маркера

```csharp
marker.Direction = new Vector3(0, 0, 1);
marker.SetDirection(new Vector3(0, 0, 1));
marker.SetDirection(0f, 0f, 1f);
```

| Метод | Описание |
|---|---|
| `SetDirection(Vector3)` | Установить направление |
| `SetDirection(float, float, float)` | Направление по координатам |

---

## Bob Up And Down (плавание)

Включает анимацию плавного движения маркера вверх-вниз. Удобно для привлечения внимания игрока.

```csharp
marker.BobUpAndDown = true;
marker.SetBobUpAndDown(true);   // alias
marker.EnableBobUpAndDown();    // alias включить
marker.DisableBobUpAndDown();   // alias выключить
```

| Метод | Описание |
|---|---|
| `SetBobUpAndDown(bool)` | Включить / выключить |
| `EnableBobUpAndDown()` | Включить |
| `DisableBobUpAndDown()` | Выключить |

### Пример

```csharp
var marker = CreateMarker(0, new Vector3(0, 0, 72), new Vector3(1, 1, 1));
marker.EnableBobUpAndDown();   // маркер плавает вверх-вниз
```

---

## Удалить маркер

```csharp
marker.Delete();
```

> Перед удалением автоматически проверяется существование маркера.

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class MarkerDemo : GlubinkaContext
{
    private Marker _shopMarker;
    private Marker _jobMarker;
    private Marker _spawnMarker;

    protected override void OnStart()
    {
        // Точка магазина — зелёное кольцо
        _shopMarker = CreateMarker(1, new Vector3(25.0f, -1345.0f, 28.0f), new Vector3(2, 2, 1));
        _shopMarker.SetColor(0, 255, 0);
        _shopMarker.EnableBobUpAndDown();

        // Точка работы — жёлтая стрелка вниз
        _jobMarker = CreateMarker(0, new Vector3(120.0f, -620.0f, 43.0f), new Vector3(1, 1, 1));
        _jobMarker.SetColor(255, 255, 0);

        // Точка спавна — синий шар
        _spawnMarker = CreateMarker(6, new Vector3(-269.0f, -955.0f, 31.0f), new Vector3(1, 1, 1));
        _spawnMarker.SetColor(0, 0, 255);
        _spawnMarker.EnableBobUpAndDown();

        Log("Маркеры созданы.");
    }

    [Command("deletemarkers")]
    public void DeleteMarkers(Client client)
    {
        var player = WrapPlayer(client);

        _shopMarker.Delete();
        _jobMarker.Delete();
        _spawnMarker.Delete();

        player.SendSuccess("Все маркеры удалены.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — создал маркер вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void Init()
    {
        var marker = CreateMarker(1, new Vector3(0, 0, 0), new Vector3(1, 1, 1));
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        var marker = CreateMarker(1, new Vector3(0, 0, 0), new Vector3(1, 1, 1));
    }
}
```

---

### Ошибка 2 — передал отрицательный размер в SetUniformScale

Неправильно:

```csharp
marker.SetUniformScale(-1f);
```

Правильно:

```csharp
marker.SetUniformScale(2f);
```

---

### Ошибка 3 — передал значение цвета вне диапазона

Неправильно:

```csharp
marker.SetColor(300, 0, 0);
```

Правильно:

```csharp
marker.SetColor(255, 0, 0);   // каждое значение от 0 до 255
```

---

### Ошибка 4 — работаешь с маркером после Delete

Неправильно:

```csharp
marker.Delete();
marker.SetColor(0, 255, 0);
```

Правильно:

```csharp
if (marker.Exists)
{
    marker.SetColor(0, 255, 0);
}
```

---

### Ошибка 5 — хранишь маркер как локальную переменную

Неправильно:

```csharp
protected override void OnStart()
{
    var marker = CreateMarker(1, new Vector3(0, 0, 72), new Vector3(1, 1, 1));
}
// маркер недоступен нигде кроме OnStart
```

Правильно — храни как поле класса:

```csharp
private Marker _marker;

protected override void OnStart()
{
    _marker = CreateMarker(1, new Vector3(0, 0, 72), new Vector3(1, 1, 1));
}
```

---

## Все методы Marker — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Handle` | Внутренний handle |
| `Exists` | Существует ли маркер |
| `Position` | Позиция (чтение / запись) |
| `Rotation` | Поворот (чтение / запись) |
| `Scale` | Масштаб (чтение / запись) |
| `Direction` | Направление (чтение / запись) |
| `Type` | Тип маркера (чтение / запись) |
| `Color` | Цвет (чтение / запись) |
| `BobUpAndDown` | Плавание (чтение / запись) |
| `Delete()` | Удалить маркер |
| `MoveTo(Vector3)` | Переместить маркер |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |
| `RotateTo(Vector3)` | Повернуть маркер |
| `RotateTo(float, float, float)` | Повернуть по координатам |
| `SetRotation(Vector3)` | Alias для RotateTo |
| `SetRotation(float, float, float)` | Alias для RotateTo |
| `SetType(int)` | Установить тип |
| `SetColor(Color)` | Цвет через объект |
| `SetColor(int, int, int, int)` | Цвет через ARGB |
| `SetColor(int, int, int)` | Цвет через RGB |
| `SetScale(Vector3)` | Масштаб через Vector3 |
| `SetScale(float, float, float)` | Масштаб по координатам |
| `SetUniformScale(float)` | Одинаковый масштаб по всем осям |
| `SetDirection(Vector3)` | Установить направление |
| `SetDirection(float, float, float)` | Направление по координатам |
| `SetBobUpAndDown(bool)` | Включить / выключить плавание |
| `EnableBobUpAndDown()` | Включить плавание |
| `DisableBobUpAndDown()` | Выключить плавание |

---

## См. также

- [Главная страница документации](https://glubinkamp.github.io/GlubinkaMP-Docs/)
- [Getting Started](https://glubinkamp.github.io/GlubinkaMP-Docs/Getting-Started.html)
- [Player](https://glubinkamp.github.io/GlubinkaMP-Docs/Player.html)
- [Players](https://glubinkamp.github.io/GlubinkaMP-Docs/Players.html)
- [Vehicle](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicle.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [Chat](https://glubinkamp.github.io/GlubinkaMP-Docs/Chat.html)
- [Blip](https://glubinkamp.github.io/GlubinkaMP-Docs/Blip.html)
- [TextLabel](https://glubinkamp.github.io/GlubinkaMP-Docs/TextLabel.html)
- [СolShape](https://glubinkamp.github.io/GlubinkaMP-Docs/СolShape.html)
- [Config](https://glubinkamp.github.io/GlubinkaMP-Docs/Config.html)
- [Logger](https://glubinkamp.github.io/GlubinkaMP-Docs/Logger.html)
- [Enums](https://glubinkamp.github.io/GlubinkaMP-Docs/Enums.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [CEF_UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
