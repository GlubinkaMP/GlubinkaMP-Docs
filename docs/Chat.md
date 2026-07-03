# Chat

Документация по работе с чатом в **Glubinka MP SDK**.

`Chat` — это статический класс для отправки сообщений игрокам: всем сразу, одному игроку, с цветом, от имени отправителя, уведомлениями и т.п.

Также здесь описан класс `ChatColor` — готовые цветовые коды для текста в чате.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Важно

`Chat` — статический класс. Его не нужно создавать через `new`.

Он доступен сразу внутри любого ресурса, унаследованного от `GlubinkaContext`.

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Chat.SendToAll("~g~Сервер запущен!");
    }
}
```

> Использовать `Chat` вне `GlubinkaContext` нельзя — он не будет инициализирован.

---

## Сообщения всем игрокам

### Простое сообщение всем

```csharp
Chat.SendToAll("Привет всем!");
```

### Цветное сообщение всем

```csharp
Chat.SendToAll("~g~", "Сервер запущен!");
Chat.SendToAll("~r~", "ВНИМАНИЕ: перезапуск через 5 минут!");
```

### Сообщение всем от имени отправителя

```csharp
Chat.SendToAllFrom("Админ", "Добро пожаловать на сервер!");
Chat.SendToAllFrom("Сервер", "Через 5 минут перезапуск!");
```

### Broadcast — alias для SendToAll

```csharp
Chat.Broadcast("Привет всем!");
Chat.Broadcast("~y~", "Внимание, событие началось!");
```

| Метод | Описание |
|---|---|
| `SendToAll(string)` | Сообщение всем |
| `SendToAll(string, string)` | Цветное сообщение всем |
| `SendToAllFrom(string, string)` | Сообщение всем от имени |
| `Broadcast(string)` | Alias для SendToAll |
| `Broadcast(string, string)` | Alias для цветного SendToAll |

### Пример

```csharp
[Command("announce")]
public void Announce(Client client, string message)
{
    Chat.SendToAllFrom("Объявление", message);
}
```

---

## Цветные сообщения всем

Готовые методы для быстрой отправки цветных сообщений всем игрокам.

```csharp
Chat.SendSuccessToAll("Событие успешно завершено.");   // зелёный
Chat.SendErrorToAll("Произошла ошибка на сервере.");   // красный
Chat.SendWarningToAll("Перезапуск через 5 минут.");    // жёлтый
Chat.SendInfoToAll("Добро пожаловать на сервер!");     // синий
```

| Метод | Цвет | Описание |
|---|---|---|
| `SendSuccessToAll(string)` | Зелёный `~g~` | Успех всем |
| `SendErrorToAll(string)` | Красный `~r~` | Ошибка всем |
| `SendWarningToAll(string)` | Жёлтый `~y~` | Предупреждение всем |
| `SendInfoToAll(string)` | Синий `~b~` | Информация всем |

### Пример

```csharp
protected override void OnStart()
{
    Chat.SendInfoToAll("Сервер запущен. Добро пожаловать!");
}
```

---

## Сообщения одному игроку

### Простое сообщение

```csharp
Chat.SendTo(player, "Привет!");
```

### Цветное сообщение

```csharp
Chat.SendTo(player, "~g~", "Ты вылечен!");
Chat.SendTo(player, "~r~", "Ты нарушил правила.");
```

### Сообщение от имени отправителя

```csharp
Chat.SendToFrom(player, "Сервер", "Добро пожаловать!");
Chat.SendToFrom(player, "Админ", "Ты предупреждён.");
```

| Метод | Описание |
|---|---|
| `SendTo(Player, string)` | Сообщение игроку |
| `SendTo(Player, string, string)` | Цветное сообщение игроку |
| `SendToFrom(Player, string, string)` | Сообщение игроку от имени |

### Пример

```csharp
[Command("pm")]
public void PrivateMessage(Client client, string targetName, string message)
{
    var player = WrapPlayer(client);

    if (!Players.TryGetByName(targetName, out Player target))
    {
        player.SendError("Игрок не найден.");
        return;
    }

    Chat.SendToFrom(target, player.Name, message);
    Chat.SendTo(player, "~y~", "Сообщение отправлено " + target.Name);
}
```

---

## Уведомления

Уведомления — это небольшие всплывающие сообщения в правом нижнем углу экрана игрока.

### Уведомление одному игроку

```csharp
Chat.Notify(player, "Ты получил оружие!");
```

### Уведомление всем

```csharp
Chat.NotifyAll("Началось событие!");
```

| Метод | Описание |
|---|---|
| `Notify(Player, string)` | Уведомление одному игроку |
| `NotifyAll(string)` | Уведомление всем игрокам |

### Цветные уведомления одному игроку

```csharp
Chat.NotifySuccess(player, "Операция выполнена.");   // зелёный
Chat.NotifyError(player, "Что-то пошло не так.");    // красный
Chat.NotifyWarning(player, "Будь осторожен.");       // жёлтый
Chat.NotifyInfo(player, "Новая информация.");        // синий
```

| Метод | Цвет | Описание |
|---|---|---|
| `NotifySuccess(Player, string)` | Зелёный `~g~` | Успех |
| `NotifyError(Player, string)` | Красный `~r~` | Ошибка |
| `NotifyWarning(Player, string)` | Жёлтый `~y~` | Предупреждение |
| `NotifyInfo(Player, string)` | Синий `~b~` | Информация |

### Цветные уведомления всем

```csharp
Chat.NotifySuccessAll("Событие завершено!");
Chat.NotifyErrorAll("Ошибка на сервере.");
Chat.NotifyWarningAll("Перезапуск через минуту.");
Chat.NotifyInfoAll("Добро пожаловать!");
```

| Метод | Цвет | Описание |
|---|---|---|
| `NotifySuccessAll(string)` | Зелёный `~g~` | Успех всем |
| `NotifyErrorAll(string)` | Красный `~r~` | Ошибка всем |
| `NotifyWarningAll(string)` | Жёлтый `~y~` | Предупреждение всем |
| `NotifyInfoAll(string)` | Синий `~b~` | Информация всем |

### Пример

```csharp
[Command("notify")]
public void NotifyCommand(Client client, string message)
{
    var player = WrapPlayer(client);
    Chat.NotifySuccessAll(message);
    player.SendSuccess("Уведомление отправлено всем.");
}
```

---

## ChatColor — цвета текста

`ChatColor` — это статический класс с готовыми цветовыми кодами для текста в чате.

Вместо того чтобы писать `"~g~"` вручную, можно использовать `ChatColor.Green` — так код читается понятнее.

### Основные цвета

```csharp
string red    = ChatColor.Red;      // ~r~
string green  = ChatColor.Green;    // ~g~
string blue   = ChatColor.Blue;     // ~b~
string white  = ChatColor.White;    // ~w~
string black  = ChatColor.Black;    // ~c~
string grey   = ChatColor.Grey;     // ~c~
string yellow = ChatColor.Yellow;   // ~y~
string orange = ChatColor.Orange;   // ~o~
string purple = ChatColor.Purple;   // ~p~
string cyan   = ChatColor.Cyan;     // ~s~
```

### Специальные коды

```csharp
string reset     = ChatColor.Reset;      // ~s~ — сброс цвета
string highlight = ChatColor.Highlight;  // ~h~ — выделение
```

### Псевдонимы

```csharp
string success = ChatColor.Success;  // ~g~ зелёный
string error   = ChatColor.Error;    // ~r~ красный
string warning = ChatColor.Warning;  // ~y~ жёлтый
string info    = ChatColor.Info;     // ~b~ синий
string def     = ChatColor.Default;  // ~w~ белый
```

### Таблица всех цветов

| Константа | Код | Цвет |
|---|---|---|
| `ChatColor.Red` | `~r~` | Красный |
| `ChatColor.Green` | `~g~` | Зелёный |
| `ChatColor.Blue` | `~b~` | Синий |
| `ChatColor.White` | `~w~` | Белый |
| `ChatColor.Black` | `~c~` | Чёрный |
| `ChatColor.Grey` | `~c~` | Серый |
| `ChatColor.Yellow` | `~y~` | Жёлтый |
| `ChatColor.Orange` | `~o~` | Оранжевый |
| `ChatColor.Purple` | `~p~` | Фиолетовый |
| `ChatColor.Cyan` | `~s~` | Голубой |
| `ChatColor.Reset` | `~s~` | Сброс цвета |
| `ChatColor.Highlight` | `~h~` | Выделение |
| `ChatColor.Success` | `~g~` | Псевдоним Green |
| `ChatColor.Error` | `~r~` | Псевдоним Red |
| `ChatColor.Warning` | `~y~` | Псевдоним Yellow |
| `ChatColor.Info` | `~b~` | Псевдоним Blue |
| `ChatColor.Default` | `~w~` | Псевдоним White |

---

## ChatColor.Colorize и ChatColor.Format

### Colorize — окрасить текст и сбросить цвет

```csharp
string msg = ChatColor.Colorize(ChatColor.Green, "Успех");
// результат: "~g~Успех~s~"
```

Удобно для вставки цветного слова внутри обычного текста:

```csharp
string msg = ChatColor.Colorize(ChatColor.Green, "Успех") + " — игрок подключился.";
Chat.SendToAll(msg);
```

### Format — форматированная строка с цветом

```csharp
string msg = ChatColor.Format(ChatColor.Green, "Игрок {0} получил {1} монет.", playerName, amount);
Chat.SendToAll(msg);
```

| Метод | Описание |
|---|---|
| `ChatColor.Colorize(string, string)` | Окрасить текст и сбросить цвет |
| `ChatColor.Format(string, string, params object[])` | Форматированная строка с цветом |

### Пример

```csharp
[Command("coins")]
public void Coins(Client client, string targetName, int amount)
{
    var player = WrapPlayer(client);

    if (!Players.TryGetByName(targetName, out Player target))
    {
        player.SendError("Игрок не найден.");
        return;
    }

    string msg = ChatColor.Format(
        ChatColor.Yellow,
        "Игрок {0} получил {1} монет.",
        target.Name,
        amount
    );

    Chat.SendToAll(msg);
}
```

---

## Использование ChatColor в сообщениях

### Простой способ — строки напрямую

```csharp
Chat.SendToAll(ChatColor.Green + "Сервер запущен!");
player.SendMessage(ChatColor.Red + "Ты нарушил правила!");
```

### Несколько цветов в одном сообщении

```csharp
player.SendMessage(
    ChatColor.Yellow + "У тебя " +
    ChatColor.White + "1000" +
    ChatColor.Yellow + " монет."
);
```

### С использованием Colorize

```csharp
string msg = "Статус: " + ChatColor.Colorize(ChatColor.Green, "Онлайн");
player.SendMessage(msg);
```

---

## Полный пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class ChatDemo : GlubinkaContext
{
    protected override void OnStart()
    {
        Chat.SendInfoToAll("Сервер запущен. Добро пожаловать!");
        Chat.NotifyInfoAll("Добро пожаловать на Glubinka MP!");
    }

    [Command("announce")]
    public void Announce(Client client, string message)
    {
        var player = WrapPlayer(client);
        Chat.SendToAllFrom("Объявление", message);
        player.SendSuccess("Объявление отправлено.");
    }

    [Command("pm")]
    public void PrivateMessage(Client client, string targetName, string message)
    {
        var player = WrapPlayer(client);

        if (!Players.TryGetByName(targetName, out Player target))
        {
            player.SendError("Игрок не найден.");
            return;
        }

        Chat.SendToFrom(target, player.Name, message);
        Chat.SendTo(player, ChatColor.Yellow, "Сообщение отправлено → " + target.Name);
    }

    [Command("warn")]
    public void Warn(Client client, string targetName, string reason)
    {
        var player = WrapPlayer(client);

        if (!Players.TryGetByName(targetName, out Player target))
        {
            player.SendError("Игрок не найден.");
            return;
        }

        Chat.NotifyError(target, "Ты получил предупреждение: " + reason);
        Chat.SendWarningToAll(
            ChatColor.Colorize(ChatColor.White, target.Name) +
            ChatColor.Warning + " получил предупреждение."
        );
    }

    [Command("restart")]
    public void Restart(Client client)
    {
        Chat.SendWarningToAll("Сервер перезапустится через 5 минут!");
        Chat.NotifyWarningAll("Перезапуск через 5 минут!");

        var player = WrapPlayer(client);
        player.SendSuccess("Объявление о перезапуске отправлено.");
    }
}
```

---

## Частые ошибки

### Ошибка 1 — использовал Chat вне GlubinkaContext

Неправильно:

```csharp
public class SomeClass
{
    public void DoSomething()
    {
        Chat.SendToAll("Привет!");
    }
}
```

Правильно:

```csharp
public class MyResource : GlubinkaContext
{
    protected override void OnStart()
    {
        Chat.SendToAll("Привет!");
    }
}
```

---

### Ошибка 2 — передал null вместо Player

Неправильно:

```csharp
Chat.SendTo(null, "Привет!");
```

Правильно:

```csharp
if (target != null)
    Chat.SendTo(target, "Привет!");
```

---

### Ошибка 3 — отправил пустое сообщение

Пустые сообщения игнорируются автоматически — `Chat` проверяет строку на пустоту.

Но лучше проверять самому:

```csharp
if (!string.IsNullOrEmpty(message))
{
    Chat.SendToAll(message);
}
```

---

### Ошибка 4 — путаешь SendTo и SendMessage

`Chat.SendTo` — это статический метод класса `Chat`, принимает `Player`.

`player.SendMessage` — это метод самого объекта `Player`.

Оба делают одно и то же, но выбирай тот, что удобнее в контексте:

```csharp
// через Chat
Chat.SendTo(player, "~g~Привет!");

// через Player напрямую
player.SendMessage("~g~Привет!");
```

---

### Ошибка 5 — забыл сбросить цвет при смешивании

Неправильно — весь текст после зелёного слова останется зелёным:

```csharp
player.SendMessage(ChatColor.Green + "Успех" + " — операция выполнена.");
```

Правильно — используй `Colorize` или `Reset`:

```csharp
player.SendMessage(ChatColor.Colorize(ChatColor.Green, "Успех") + " — операция выполнена.");
// или вручную:
player.SendMessage(ChatColor.Green + "Успех" + ChatColor.Reset + " — операция выполнена.");
```

---

## Все методы Chat — сводная таблица

| Метод | Описание |
|---|---|
| `SendToAll(string)` | Сообщение всем |
| `SendToAll(string, string)` | Цветное сообщение всем |
| `SendToAllFrom(string, string)` | Сообщение всем от имени |
| `Broadcast(string)` | Alias для SendToAll |
| `Broadcast(string, string)` | Alias для цветного SendToAll |
| `SendSuccessToAll(string)` | Зелёное сообщение всем |
| `SendErrorToAll(string)` | Красное сообщение всем |
| `SendWarningToAll(string)` | Жёлтое сообщение всем |
| `SendInfoToAll(string)` | Синее сообщение всем |
| `SendTo(Player, string)` | Сообщение одному игроку |
| `SendTo(Player, string, string)` | Цветное сообщение одному |
| `SendToFrom(Player, string, string)` | Сообщение одному от имени |
| `Notify(Player, string)` | Уведомление одному игроку |
| `NotifyAll(string)` | Уведомление всем |
| `NotifySuccess(Player, string)` | Зелёное уведомление одному |
| `NotifyError(Player, string)` | Красное уведомление одному |
| `NotifyWarning(Player, string)` | Жёлтое уведомление одному |
| `NotifyInfo(Player, string)` | Синее уведомление одному |
| `NotifySuccessAll(string)` | Зелёное уведомление всем |
| `NotifyErrorAll(string)` | Красное уведомление всем |
| `NotifyWarningAll(string)` | Жёлтое уведомление всем |
| `NotifyInfoAll(string)` | Синее уведомление всем |

---

## Все константы ChatColor — сводная таблица

| Константа | Код | Описание |
|---|---|---|
| `ChatColor.Red` | `~r~` | Красный |
| `ChatColor.Green` | `~g~` | Зелёный |
| `ChatColor.Blue` | `~b~` | Синий |
| `ChatColor.White` | `~w~` | Белый |
| `ChatColor.Black` | `~c~` | Чёрный |
| `ChatColor.Grey` | `~c~` | Серый |
| `ChatColor.Yellow` | `~y~` | Жёлтый |
| `ChatColor.Orange` | `~o~` | Оранжевый |
| `ChatColor.Purple` | `~p~` | Фиолетовый |
| `ChatColor.Cyan` | `~s~` | Голубой |
| `ChatColor.Reset` | `~s~` | Сброс цвета |
| `ChatColor.Highlight` | `~h~` | Выделение |
| `ChatColor.Success` | `~g~` | Псевдоним Green |
| `ChatColor.Error` | `~r~` | Псевдоним Red |
| `ChatColor.Warning` | `~y~` | Псевдоним Yellow |
| `ChatColor.Info` | `~b~` | Псевдоним Blue |
| `ChatColor.Default` | `~w~` | Псевдоним White |

## Все методы ChatColor — сводная таблица

| Метод | Описание |
|---|---|
| `ChatColor.Colorize(string, string)` | Окрасить текст и сбросить цвет |
| `ChatColor.Format(string, string, params object[])` | Форматированная строка с цветом |

---

## См. также
