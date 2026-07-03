# TextLabel

Документация по работе с 3D-текстовыми метками в **Glubinka MP SDK**.

TextLabel — это надпись в игровом мире, которая висит в определённой точке и видна игрокам поблизости. Используется для подписей над входами, точками работы, NPC, объектами и любыми другими местами.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как создать TextLabel

TextLabel создаётся через метод `CreateTextLabel` из `GlubinkaContext`.

```csharp
var label = CreateTextLabel(text, position, range);
```

- `text` — текст надписи
- `position` — позиция в мире
- `range` — дальность видимости (по умолчанию `10f`)

### Пример

```csharp
protected override void OnStart()
{
    var label = CreateTextLabel("Вход в магазин", new Vector3(25.0f, -1345.0f, 29.0f));
}
```

### С указанием дальности

```csharp
var label = CreateTextLabel("Банк", new Vector3(149.0f, -1040.0f, 29.0f), 15f);
```

---

## Основные свойства

```csharp
NetHandle handle = label.Handle;
bool exists      = label.Exists;
Vector3 position = label.Position;
string text      = label.Text;
float range      = label.Range;
Color color      = label.Color;
bool seeThrough  = label.SeeThrough;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Handle` | `NetHandle` | Внутренний handle метки |
| `Exists` | `bool` | Существует ли метка |
| `Position` | `Vector3` | Позиция в мире (чтение / запись) |
| `Text` | `string` | Текст надписи (чтение / запись) |
| `Range` | `float` | Дальность видимости (чтение / запись) |
| `Color` | `Color` | Цвет текста (чтение / запись) |
| `SeeThrough` | `bool` | Видна ли через стены (чтение / запись) |

---

## Текст метки

### Установить текст

```csharp
label.Text = "Новый текст";
label.SetText("Новый текст");   // alias
```

### Добавить текст к существующему

```csharp
label.Append(" | Открыто");
```

Если текст метки был `"Магазин"` — после станет `"Магазин | Открыто"`.

### Очистить текст

```csharp
label.Clear();
```

| Метод | Описание |
|---|---|
| `SetText(string)` | Установить текст |
| `Append(string)` | Добавить к текущему тексту |
| `Clear()` | Очистить текст |

### Пример

```csharp
var label = CreateTextLabel("Магазин", new Vector3(25.0f, -1345.0f, 29.0f));
label.Append("\n~g~Открыто");   // добавить статус
```

---

## Позиция метки

```csharp
label.Position = new Vector3(100, 200, 30);
label.MoveTo(new Vector3(100, 200, 30));
label.MoveTo(100f, 200f, 30f);
label.SetPosition(new Vector3(100, 200, 30));   // alias
label.SetPosition(100f, 200f, 30f);             // alias
```

| Метод | Описание |
|---|---|
| `MoveTo(Vector3)` | Переместить метку |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |

---

## Дальность видимости

```csharp
label.Range = 20f;
label.SetRange(20f);   // alias
```

> Значение по умолчанию при создании через `CreateTextLabel` — `10f`.
> Range не может быть меньше `0` — SDK выбросит ошибку.

### Пример

```csharp
var label = CreateTextLabel("Аэропорт", new Vector3(-1034.0f, -2733.0f, 20.0f), 25f);
```

---

## Цвет текста

### Через объект Color

```csharp
label.Color = new Color(255, 255, 255, 255);
label.SetColor(new Color(255, 255, 255, 255));
```

### Через компоненты RGB

```csharp
label.SetColor(255, 255, 255);          // белый, alpha = 255
label.SetColor(255, 0, 0, 255);         // красный, alpha = 255
label.SetColor(0, 255, 0, 200);         // зелёный, полупрозрачный
```

| Метод | Описание |
|---|---|
| `SetColor(Color)` | Цвет через объект Color |
| `SetColor(int, int, int)` | Цвет через RGB, alpha = 255 |
| `SetColor(int, int, int, int)` | Цвет через RGBA |

### Популярные цвета

| Вызов | Цвет |
|---|---|
| `SetColor(255, 255, 255)` | Белый |
| `SetColor(255, 0, 0)` | Красный |
| `SetColor(0, 255, 0)` | Зелёный |
| `SetColor(0, 0, 255)` | Синий |
| `SetColor(255, 255, 0)` | Жёлтый |
| `SetColor(255, 165, 0)` | Оранжевый |

### Пример

```csharp
var label = CreateTextLabel("Больница", new Vector3(295.0f, -584.0f, 43.0f));
label.SetColor(0, 255, 0);   // зелёный текст
```

> ⚠️ Чтение свойства `Color` может быть неточным из-за особенности ядра. Запись работает корректно.

---

## See Through (видимость через стены)

Если включено — текст метки виден даже через стены и объекты.

```csharp
label.SeeThrough = true;
label.SetSeeThrough(true);   // alias
label.EnableSeeThrough();    // alias включить
label.DisableSeeThrough();   // alias выключить
```

| Метод | Описание |
|---|---|
| `SetSeeThrough(bool)` | Включить / выключить |
| `EnableSeeThrough()` | Включить |
| `DisableSeeThrough()` | Выключить |

### Пример

```csharp
var label = CreateTextLabel("Подвал", new Vector3(0, 0, 10.0f));
label.EnableSeeThrough();   // видно даже через стены
```

---

## Удалить метку

```csharp
label.Delete();
```

> Перед удалением автоматически проверяется существование метки.

---

## Цветной текст в метке

Внутри текста метки можно использовать те же цветовые коды, что и в чате.

```csharp
var label = CreateTextLabel("~g~Открыто", new Vector3(25.0f, -1345.0f, 29.0f));
```

```csharp
var label = CreateTextLabel(
    "~y~Магазин\n~g~Открыто",
    new Vector3(25.0f, -1345.0f, 29.0f)
);
```

### Коды цветов

| Код | Цвет |
|---|---|
| `~r~` | Красный |
| `~g~` | Зелёный |
| `~b~` | Синий |
| `~y~` | Жёлтый |
| `~w~` | Белый |
| `~p~` | Фиолетовый |
| `~o~` | Оранжевый |

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class TextLabelDemo : GlubinkaContext
{
    private TextLabel _shopLabel;
    private TextLabel _bankLabel;
    private TextLabel _hospitalLabel;

    protected override void OnStart()
    {
        // Магазин
        _shopLabel = CreateTextLabel(
            "~y~Оружейный магазин\n~g~Открыто",
            new Vector3(25.0f, -1345.0f, 29.5f),
            10f
        );
        _shopLabel.SetColor(255, 255, 255);

        // Банк
        _bankLabel = CreateTextLabel(
            "~b~Банк\n~w~Нажми ~y~E ~w~для входа",
            new Vector3(149.0f, -1040.0f, 29.5f),
            8f
        );

        // Больница
        _hospitalLabel = CreateTextLabel(
            "~g~Больница",
            new Vector3(295.0f, -584.0f, 43.5f),
            15f
        );
        _hospitalLabel.SetColor(0, 255, 0);
        _hospitalLabel.EnableSeeThrough();

        Log("TextLabel-ы созданы.");
    }

    [Command("shopopen")]
    public void ShopOpen(Client client)
    {
        var player = WrapPlayer(client);
        _shopLabel.Clear();
        _shopLabel.SetText("~y~Оружейный магазин\n~g~Открыто");
        player.SendSuccess("Магазин открыт.");
    }

    [Command("shopclose")]
    public void ShopClose(Client client)
    {
        var player = WrapPlayer(client);
        _shopLabel.Clear();
        _shopLabel.SetText("~y~Оружейный магазин\n~r~Закрыто");
        player.SendMessage("~r~Магазин закрыт.");
    }

    [Command("deletelabels")]
    public void DeleteLabels(Client client)
    {
        var player = WrapPlayer(client);
        _shopLabel.Delete();
        _bankLabel.Delete();
        _hospitalLabel.Delete();
        player.SendSuccess("Все метки удалены.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — создал TextLabel вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void Init()
    {
        var label = CreateTextLabel("Текст", new Vector3(0, 0, 0));
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        var label = CreateTextLabel("Текст", new Vector3(0, 0, 0));
    }
}
```

---

### Ошибка 2 — передал отрицательный Range

Неправильно:

```csharp
label.SetRange(-5f);
```

Правильно:

```csharp
label.SetRange(10f);
```

---

### Ошибка 3 — передал значение цвета вне диапазона

Неправильно:

```csharp
label.SetColor(300, 0, 0);
```

Правильно:

```csharp
label.SetColor(255, 0, 0);   // каждое значение от 0 до 255
```

---

### Ошибка 4 — работаешь с меткой после Delete

Неправильно:

```csharp
label.Delete();
label.SetText("Что-то");
```

Правильно:

```csharp
if (label.Exists)
{
    label.SetText("Что-то");
}
```

---

### Ошибка 5 — хранишь метку как локальную переменную

Неправильно:

```csharp
protected override void OnStart()
{
    var label = CreateTextLabel("Текст", new Vector3(0, 0, 72));
}
// label недоступен нигде кроме OnStart
```

Правильно — храни как поле класса:

```csharp
private TextLabel _label;

protected override void OnStart()
{
    _label = CreateTextLabel("Текст", new Vector3(0, 0, 72));
}
```

---

### Ошибка 6 — не учёл перенос строки в тексте

Для многострочного текста используй `\n`:

```csharp
var label = CreateTextLabel("Магазин\nОткрыто", new Vector3(0, 0, 72));
```

Или через `Append`:

```csharp
var label = CreateTextLabel("Магазин", new Vector3(0, 0, 72));
label.Append("\nОткрыто");
```

---

## Все методы TextLabel — сводная таблица

| Метод / Свойство | Описание |
|---|---|
| `Handle` | Внутренний handle |
| `Exists` | Существует ли метка |
| `Position` | Позиция (чтение / запись) |
| `Text` | Текст (чтение / запись) |
| `Range` | Дальность видимости (чтение / запись) |
| `Color` | Цвет (чтение / запись) |
| `SeeThrough` | Видна через стены (чтение / запись) |
| `Delete()` | Удалить метку |
| `MoveTo(Vector3)` | Переместить метку |
| `MoveTo(float, float, float)` | Переместить по координатам |
| `SetPosition(Vector3)` | Alias для MoveTo |
| `SetPosition(float, float, float)` | Alias для MoveTo |
| `SetText(string)` | Установить текст |
| `Append(string)` | Добавить к тексту |
| `Clear()` | Очистить текст |
| `SetRange(float)` | Установить дальность видимости |
| `SetColor(Color)` | Цвет через объект |
| `SetColor(int, int, int)` | Цвет через RGB |
| `SetColor(int, int, int, int)` | Цвет через RGBA |
| `SetSeeThrough(bool)` | Включить / выключить see through |
| `EnableSeeThrough()` | Включить see through |
| `DisableSeeThrough()` | Выключить see through |

---

## См. также
