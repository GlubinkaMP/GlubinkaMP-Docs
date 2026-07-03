# Player

Документация по работе с игроками в **Glubinka MP SDK**.

Этот файл описывает именно удобный SDK-слой `GlubinkaMP.SDK`, а не низкоуровневые методы ядра.
Если ты пишешь ресурс через `Еще не добавлено сюда`, то обычно работаешь именно с классом `Player`.

---

## Что такое Player

`Player` — это публичная обёртка над внутренним объектом игрока.

Она нужна для того, чтобы разработчику ресурса не приходилось постоянно вызывать низкоуровневые методы API вручную.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как получить Player

В командах и событиях ядро часто передаёт `Client`.
Чтобы превратить его в `Player`, используй метод `WrapPlayer(client)` из `GlubinkaContext`.

### Пример

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;

public class PlayerExamples : GlubinkaContext
{
    [Command("whoami")]
    public void WhoAmI(Client client)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~g~Твоё имя: ~w~" + player.Name);
    }
}
```

---

## Основные свойства

```csharp
string name = player.Name;
string socialClub = player.SocialClubName;
Vector3 position = player.Position;
Vector3 rotation = player.Rotation;
Vector3 velocity = player.Velocity;
int health = player.Health;
int armor = player.Armor;
int ping = player.Ping;
bool inVehicle = player.IsInVehicle;
int currentWeapon = player.CurrentWeapon;
int modelHash = player.ModelHash;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Name` | `string` | Ник игрока |
| `SocialClubName` | `string` | Имя Social Club |
| `Position` | `Vector3` | Текущая позиция |
| `Rotation` | `Vector3` | Текущий поворот |
| `Velocity` | `Vector3` | Текущая скорость |
| `Health` | `int` | Здоровье (0–200) |
| `Armor` | `int` | Броня (0–100) |
| `Ping` | `int` | Пинг в миллисекундах |
| `IsInVehicle` | `bool` | Находится ли в транспорте |
| `CurrentWeapon` | `int` | Хэш текущего оружия |
| `ModelHash` | `int` | Хэш модели персонажа |

---

## Свойства состояния

```csharp
bool connected = player.IsConnected;
bool dead = player.IsDead;
bool respawning = player.IsRespawning;
bool onFire = player.IsOnFire;
bool parachuting = player.IsParachuting;
bool freefall = player.IsInFreefall;
bool aiming = player.IsAiming;
bool shooting = player.IsShooting;
bool reloading = player.IsReloading;
bool inCover = player.IsInCover;
bool onLadder = player.IsOnLadder;
bool spectating = player.IsSpectating;
```

| Свойство | Тип | Описание |
|---|---|---|
| `IsConnected` | `bool` | Подключён ли к серверу |
| `IsDead` | `bool` | Мёртв ли игрок |
| `IsRespawning` | `bool` | Респавнится ли сейчас |
| `IsOnFire` | `bool` | Горит ли игрок |
| `IsParachuting` | `bool` | Летит ли на парашюте |
| `IsInFreefall` | `bool` | В свободном падении |
| `IsAiming` | `bool` | Целится ли |
| `IsShooting` | `bool` | Стреляет ли |
| `IsReloading` | `bool` | Перезаряжает ли оружие |
| `IsInCover` | `bool` | Находится ли в укрытии |
| `IsOnLadder` | `bool` | Находится ли на лестнице |
| `IsSpectating` | `bool` | В режиме наблюдателя |

---

## Дополнительные свойства

```csharp
Vector3 aimPos = player.AimPosition;
string address = player.Address;
int team = player.Team;
int wanted = player.WantedLevel;
int seat = player.VehicleSeat;
NetHandle vehicle = player.CurrentVehicle;
bool seatbelt = player.Seatbelt;
```

| Свойство | Тип | Описание |
|---|---|---|
| `AimPosition` | `Vector3` | Точка, в которую целится игрок |
| `Address` | `string` | IP-адрес игрока |
| `Team` | `int` | Команда (team) |
| `WantedLevel` | `int` | Уровень розыска (0–5) |
| `VehicleSeat` | `int` | Сиденье в транспорте (-1 = водитель, -3 = не в машине) |
| `CurrentVehicle` | `NetHandle` | Handle транспорта, в котором сидит |
| `Seatbelt` | `bool` | Пристёгнут ли ремень безопасности |

---

## Nametag (надпись над головой)

### Свойства (чтение)

```csharp
string tag = player.Nametag;
bool visible = player.NametagVisible;
Color color = player.NametagColor;
```

| Свойство | Тип | Описание |
|---|---|---|
| `Nametag` | `string` | Текст nametag |
| `NametagVisible` | `bool` | Виден ли nametag |
| `NametagColor` | `Color` | Цвет nametag |

### Методы

```csharp
player.SetNametag("Admin");
player.ResetNametag();
player.SetNametagVisible(true);
player.SetNametagVisible(false);
player.SetNametagColor(new Color(255, 0, 0));
player.ResetNametagColor();
```

| Метод | Описание |
|---|---|
| `SetNametag(string text)` | Установить текст nametag |
| `ResetNametag()` | Сбросить к стандартному |
| `SetNametagVisible(bool)` | Показать или скрыть |
| `SetNametagColor(Color)` | Установить цвет |
| `ResetNametagColor()` | Сбросить цвет к стандартному |

### Пример

```csharp
[Command("settag")]
public void SetTag(Client client, string text)
{
    var player = WrapPlayer(client);
    player.SetNametag(text);
    player.SetNametagColor(new Color(0, 255, 0));
    player.SendMessage("~g~Nametag изменён.");
}
```

---

## Здоровье

### Установить здоровье

```csharp
player.SetHealth(100);
```

### Убить игрока

```csharp
player.Kill();
```

### Пример

```csharp
[Command("healme")]
public void HealMe(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
    player.SendMessage("~g~Ты вылечен.");
}
```

> Допустимое значение: от `0` до `200`.

---

## Броня

### Установить броню

```csharp
player.SetArmor(100);
```

### Пример

```csharp
[Command("armor")]
public void ArmorCommand(Client client)
{
    var player = WrapPlayer(client);
    player.SetArmor(100);
    player.SendMessage("~b~Броня выдана.");
}
```

> Допустимое значение: от `0` до `100`.

---

## Позиция и телепорт

### Телепортировать по `Vector3`

```csharp
player.Teleport(new Vector3(-269.0f, -955.0f, 31.0f));
```

### Телепортировать по координатам

```csharp
player.Teleport(-269.0f, -955.0f, 31.0f);
```

### Пример

```csharp
[Command("gotoairport")]
public void GoToAirport(Client client)
{
    var player = WrapPlayer(client);
    player.Teleport(-1034.0f, -2733.0f, 20.0f);
    player.SendMessage("~y~Ты телепортирован в аэропорт.");
}
```

---

## Скорость и заморозка

### Установить скорость

```csharp
player.SetVelocity(new Vector3(0, 0, 50.0f));   // подбросить вверх
player.SetVelocity(0, 0, 50.0f);                 // то же самое
```

### Заморозить / разморозить

```csharp
player.Freeze();        // заморозить
player.Freeze(true);    // заморозить
player.Freeze(false);   // разморозить
```

### Пример

```csharp
[Command("launch")]
public void Launch(Client client)
{
    var player = WrapPlayer(client);
    player.SetVelocity(0, 0, 80.0f);
    player.SendMessage("~o~Полетели!");
}
```

---

## Скин персонажа

### Установить скин по `PedHash`

```csharp
player.SetSkin(PedHash.Cop01SFY);
```

### Установить скин по числовому хэшу

```csharp
player.SetSkin((int)PedHash.Fireman01SMY);
```

### Популярные модели

| PedHash | Описание |
|---|---|
| `PedHash.Michael` | Майкл |
| `PedHash.Franklin` | Франклин |
| `PedHash.Trevor` | Тревор |
| `PedHash.Cop01SFY` | Полицейский |
| `PedHash.Doctor01SMM` | Доктор |
| `PedHash.Fireman01SMY` | Пожарный |
| `PedHash.Paramedic01SMM` | Парамедик |
| `PedHash.FreemodeMale01` | Фримод мужчина |
| `PedHash.FreemodeFemale01` | Фримод женщина |

### Пример

```csharp
[Command("copskin")]
public void CopSkin(Client client)
{
    var player = WrapPlayer(client);
    player.SetSkin(PedHash.Cop01SFY);
    player.SendMessage("~b~Твой скин изменён.");
}
```

---

## Оружие (базовое)

### Выдать оружие

```csharp
player.GiveWeapon(WeaponHash.Pistol, 100);
```

### Забрать одно оружие

```csharp
player.RemoveWeapon(WeaponHash.Pistol);
```

### Забрать всё оружие

```csharp
player.RemoveAllWeapons();
```

### Получить список всех оружий

```csharp
WeaponHash[] weapons = player.GetWeapons();
```

### Популярные оружия

| WeaponHash | Описание |
|---|---|
| `WeaponHash.Pistol` | Пистолет |
| `WeaponHash.CombatPistol` | Боевой пистолет |
| `WeaponHash.SMG` | SMG |
| `WeaponHash.AssaultRifle` | Штурмовая винтовка |
| `WeaponHash.CarbineRifle` | Карабин |
| `WeaponHash.SniperRifle` | Снайперская винтовка |
| `WeaponHash.Shotgun` | Дробовик |
| `WeaponHash.Knife` | Нож |
| `WeaponHash.Bat` | Бита |

### Пример

```csharp
[Command("pistol")]
public void PistolCommand(Client client)
{
    var player = WrapPlayer(client);
    player.GiveWeapon(WeaponHash.Pistol, 60);
    player.SendMessage("~g~Ты получил пистолет.");
}
```

---

## Оружие (расширенное)

### Патроны

```csharp
player.SetWeaponAmmo(WeaponHash.Pistol, 200);
int ammo = player.GetWeaponAmmo(WeaponHash.Pistol);
```

### Окраска (tint)

```csharp
player.SetWeaponTint(WeaponHash.Pistol, WeaponTint.Gold);
WeaponTint tint = player.GetWeaponTint(WeaponHash.Pistol);
```

### Компоненты (глушитель, прицел и т.п.)

```csharp
player.GiveWeaponComponent(WeaponHash.AssaultRifle, WeaponComponent.AtArSupp02);
player.RemoveWeaponComponent(WeaponHash.AssaultRifle, WeaponComponent.AtArSupp02);
bool has = player.HasWeaponComponent(WeaponHash.AssaultRifle, WeaponComponent.AtArSupp02);
WeaponComponent[] comps = player.GetWeaponComponents(WeaponHash.AssaultRifle);
```

### Взорвать липкие бомбы

```csharp
player.DetonateStickies();
```

### Пример

```csharp
[Command("goldpistol")]
public void GoldPistol(Client client)
{
    var player = WrapPlayer(client);
    player.GiveWeapon(WeaponHash.Pistol, 100);
    player.SetWeaponTint(WeaponHash.Pistol, WeaponTint.Gold);
    player.SendMessage("~o~Ты получил золотой пистолет!");
}
```

---

## Одежда

### Установить одежду

```csharp
player.SetClothes(slot, drawable, texture);
```

### Прочитать одежду

```csharp
int drawable = player.GetClothesDrawable(slot);
int texture = player.GetClothesTexture(slot);
```

### Сбросить к стандартной

```csharp
player.SetDefaultClothes();
```

### Слоты одежды

| Слот | Описание |
|---|---|
| 0 | Голова |
| 1 | Маска |
| 2 | Причёска |
| 3 | Торс |
| 4 | Ноги |
| 5 | Сумка / парашют |
| 6 | Ступни |
| 7 | Аксессуар шеи |
| 8 | Рубашка / бронежилет |
| 9 | Бронежилет (отдельно) |
| 10 | Наклейки / значки |
| 11 | Верхняя одежда |

### Пример

```csharp
[Command("hat")]
public void Hat(Client client)
{
    var player = WrapPlayer(client);
    player.SetClothes(0, 45, 0);
    player.SendMessage("~g~Шляпа надета.");
}
```

---

## Аксессуары

### Установить аксессуар

```csharp
player.SetAccessory(slot, drawable, texture);
```

### Убрать аксессуар

```csharp
player.ClearAccessory(slot);
```

### Прочитать аксессуар

```csharp
int drawable = player.GetAccessoryDrawable(slot);
int texture = player.GetAccessoryTexture(slot);
```

### Слоты аксессуаров

| Слот | Описание |
|---|---|
| 0 | Шляпа / головной убор |
| 1 | Очки |
| 2 | Уши (наушники и т.п.) |

### Пример

```csharp
[Command("glasses")]
public void Glasses(Client client)
{
    var player = WrapPlayer(client);
    player.SetAccessory(1, 5, 0);
    player.SendMessage("~g~Очки надеты.");
}
```

---

## Анимации и сценарии

### Проиграть анимацию

```csharp
player.PlayAnimation("amb@world_human_hang_out_street@male_b@base", "base");
player.PlayAnimation("anim@mp_player_intcelebrationmale@wave", "wave", 1);  // зацикленная
```

### Проиграть сценарий

```csharp
player.PlayScenario("WORLD_HUMAN_SMOKING");
```

### Остановить анимацию

```csharp
player.StopAnimation();
```

### Очистить задачи

```csharp
player.ClearTasks();
```

### Популярные сценарии

| Сценарий | Описание |
|---|---|
| `WORLD_HUMAN_SMOKING` | Курение |
| `WORLD_HUMAN_DRINKING` | Питьё |
| `WORLD_HUMAN_STAND_MOBILE` | Стоит с телефоном |
| `WORLD_HUMAN_COP_IDLES` | Полицейский стоит |
| `WORLD_HUMAN_GUARD_STAND` | Охранник стоит |
| `WORLD_HUMAN_MUSICIAN` | Музыкант |

### Пример

```csharp
[Command("smoke")]
public void Smoke(Client client)
{
    var player = WrapPlayer(client);
    player.PlayScenario("WORLD_HUMAN_SMOKING");
    player.SendMessage("~w~Ты закурил.");
}

[Command("stopanim")]
public void StopAnim(Client client)
{
    var player = WrapPlayer(client);
    player.StopAnimation();
    player.ClearTasks();
    player.SendMessage("~w~Анимация остановлена.");
}
```

---

## Spectate (наблюдение)

### Перейти в режим свободной камеры

```csharp
player.SetSpectator();
```

### Наблюдать за другим игроком

```csharp
player.SpectatePlayer(targetPlayer);
```

### Выйти из режима наблюдения

```csharp
player.Unspectate();
```

### Пример

```csharp
[Command("spec")]
public void Spec(Client client, string targetName)
{
    var player = WrapPlayer(client);
    Client targetClient = API.getPlayerFromName(targetName);

    if (targetClient == null)
    {
        player.SendMessage("~r~Игрок не найден.");
        return;
    }

    var target = WrapPlayer(targetClient);
    player.SpectatePlayer(target);
    player.SendMessage("~y~Наблюдаешь за " + target.Name);
}

[Command("unspec")]
public void Unspec(Client client)
{
    var player = WrapPlayer(client);
    player.Unspectate();
    player.SendMessage("~g~Наблюдение окончено.");
}
```

---

## Команда и розыск

### Установить команду

```csharp
player.SetTeam(1);
```

### Установить уровень розыска

```csharp
player.SetWantedLevel(3);
```

> Допустимые значения розыска: от `0` до `5`.

### Пример

```csharp
[Command("wanted")]
public void Wanted(Client client, int level)
{
    var player = WrapPlayer(client);
    player.SetWantedLevel(level);
    player.SendMessage("~o~Уровень розыска: " + level);
}
```

---

## Ремень безопасности

### Пристегнуть / отстегнуть

```csharp
player.SetSeatbelt(true);    // пристегнуть
player.SetSeatbelt(false);   // отстегнуть
```

### Проверить

```csharp
bool buckled = player.Seatbelt;
```

---

## Имя игрока

### Сменить имя

```csharp
player.SetName("NewNickname");
```

> Имя не может быть пустым.

### Пример

```csharp
[Command("rename")]
public void Rename(Client client, string newName)
{
    var player = WrapPlayer(client);
    player.SetName(newName);
    player.SendMessage("~g~Имя изменено на: " + newName);
}
```

---

## Кик и бан

### Кикнуть игрока

```csharp
player.Kick();
player.Kick("Нарушение правил");
```

### Забанить игрока

```csharp
player.Ban();
player.Ban("Читы");
```

### Пример

```csharp
[Command("kickplayer")]
public void KickPlayer(Client client, string targetName, string reason)
{
    Client targetClient = API.getPlayerFromName(targetName);

    if (targetClient == null)
    {
        var player = WrapPlayer(client);
        player.SendMessage("~r~Игрок не найден.");
        return;
    }

    var target = WrapPlayer(targetClient);
    target.Kick(reason);
}
```

---

## Работа с транспортом

### Посадить игрока в транспорт

```csharp
player.PutIntoVehicle(vehicle);         // водитель
player.PutIntoVehicle(vehicle, 0);      // передний пассажир
```

### Значения сидений

| Seat | Описание |
|---|---|
| `-1` | Водитель |
| `0` | Передний пассажир |
| `1` | Заднее место |
| `2` | Следующее заднее место |

### Высадить игрока из транспорта

```csharp
player.RemoveFromVehicle();
player.WarpOutOfVehicle();   // альтернативное имя, делает то же самое
```

---

## Сообщения игроку

### Простое сообщение

```csharp
player.SendMessage("Привет!");
```

### Цветное сообщение

```csharp
player.SendMessage("~g~", "Добро пожаловать!");
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

public class PlayerDemo : GlubinkaContext
{
    [Command("starterkit")]
    public void StarterKit(Client client)
    {
        var player = WrapPlayer(client);

        player.SetHealth(100);
        player.SetArmor(100);
        player.SetSkin(PedHash.Cop01SFY);
        player.GiveWeapon(WeaponHash.Pistol, 90);
        player.GiveWeapon(WeaponHash.AssaultRifle, 200);
        player.SetWeaponTint(WeaponHash.Pistol, WeaponTint.Gold);
        player.SetClothes(1, 35, 0);
        player.SendMessage("~g~Ты получил стартовый набор.");
    }

    [Command("killme")]
    public void KillMe(Client client)
    {
        var player = WrapPlayer(client);
        player.Kill();
    }

    [Command("spawncity")]
    public void SpawnCity(Client client)
    {
        var player = WrapPlayer(client);
        player.Teleport(-269.0f, -955.0f, 31.0f);
        player.SendMessage("~y~Ты перемещён в город.");
    }

    [Command("smoke")]
    public void Smoke(Client client)
    {
        var player = WrapPlayer(client);
        player.PlayScenario("WORLD_HUMAN_SMOKING");
    }

    [Command("stopanim")]
    public void StopAnim(Client client)
    {
        var player = WrapPlayer(client);
        player.StopAnimation();
        player.ClearTasks();
    }

    [Command("freeze")]
    public void FreezeMe(Client client)
    {
        var player = WrapPlayer(client);
        player.Freeze();
        player.SendMessage("~b~Ты заморожен.");
    }

    [Command("unfreeze")]
    public void UnfreezeMe(Client client)
    {
        var player = WrapPlayer(client);
        player.Freeze(false);
        player.SendMessage("~g~Ты разморожен.");
    }
}
```

---

## Как получить Player для другого игрока

```csharp
Client targetClient = API.getPlayerFromName("Ivan");

if (targetClient == null)
{
    return;
}

var target = WrapPlayer(targetClient);
target.SendMessage("~g~Тебя нашли.");
```

---

## Частые ошибки

### Ошибка 1 — забыл обернуть `Client` в `Player`

Неправильно:

```csharp
public void HealMe(Client client)
{
    client.SetHealth(100);
}
```

Правильно:

```csharp
public void HealMe(Client client)
{
    var player = WrapPlayer(client);
    player.SetHealth(100);
}
```

---

### Ошибка 2 — передал неправильное значение здоровья

Неправильно:

```csharp
player.SetHealth(999);
```

Правильно:

```csharp
player.SetHealth(100);
```

---

### Ошибка 3 — работаешь с `Vehicle`, но передал `null`

Неправильно:

```csharp
player.PutIntoVehicle(null);
```

Правильно:

```csharp
var vehicle = CreateVehicle(VehicleHash.Adder, player.Position, new Vector3(0, 0, 0));
player.PutIntoVehicle(vehicle);
```

---

### Ошибка 4 — думаешь, что `CurrentWeapon` уже `WeaponHash`

Свойство `CurrentWeapon` возвращает `int`.

Если нужен enum:

```csharp
WeaponHash weapon = (WeaponHash)player.CurrentWeapon;
```

---

### Ошибка 5 — розыск больше 5

Неправильно:

```csharp
player.SetWantedLevel(10);
```

Правильно:

```csharp
player.SetWantedLevel(5);
```

---

### Ошибка 6 — пустое имя

Неправильно:

```csharp
player.SetName("");
```

Правильно:

```csharp
player.SetName("NewName");
```

---

## Все методы Player — сводная таблица

| Метод | Описание |
|---|---|
| `SetHealth(int)` | Установить здоровье (0–200) |
| `SetArmor(int)` | Установить броню (0–100) |
| `Kill()` | Убить игрока |
| `Teleport(Vector3)` | Телепортировать |
| `Teleport(float, float, float)` | Телепортировать по координатам |
| `SetVelocity(Vector3)` | Установить скорость |
| `SetVelocity(float, float, float)` | Установить скорость по координатам |
| `Freeze(bool)` | Заморозить / разморозить |
| `SetSkin(PedHash)` | Сменить скин |
| `SetSkin(int)` | Сменить скин по хэшу |
| `GiveWeapon(WeaponHash, int)` | Выдать оружие |
| `RemoveWeapon(WeaponHash)` | Убрать оружие |
| `RemoveAllWeapons()` | Убрать все оружия |
| `GetWeapons()` | Получить список оружий |
| `SetWeaponAmmo(WeaponHash, int)` | Установить патроны |
| `GetWeaponAmmo(WeaponHash)` | Получить кол-во патронов |
| `SetWeaponTint(WeaponHash, WeaponTint)` | Установить окраску оружия |
| `GetWeaponTint(WeaponHash)` | Получить окраску оружия |
| `GiveWeaponComponent(...)` | Добавить компонент |
| `RemoveWeaponComponent(...)` | Убрать компонент |
| `HasWeaponComponent(...)` | Проверить компонент |
| `GetWeaponComponents(...)` | Получить все компоненты |
| `DetonateStickies()` | Взорвать липкие бомбы |
| `PutIntoVehicle(Vehicle, int)` | Посадить в транспорт |
| `RemoveFromVehicle()` | Высадить из транспорта |
| `WarpOutOfVehicle()` | Высадить из транспорта |
| `SetTeam(int)` | Установить команду |
| `SetWantedLevel(int)` | Установить розыск (0–5) |
| `SetSeatbelt(bool)` | Пристегнуть / отстегнуть ремень |
| `SetName(string)` | Сменить имя |
| `Kick(string)` | Кикнуть |
| `Ban(string)` | Забанить |
| `PlayAnimation(string, string, int)` | Проиграть анимацию |
| `PlayScenario(string)` | Проиграть сценарий |
| `StopAnimation()` | Остановить анимацию |
| `ClearTasks()` | Очистить задачи |
| `SetNametag(string)` | Установить nametag |
| `ResetNametag()` | Сбросить nametag |
| `SetNametagVisible(bool)` | Показать / скрыть nametag |
| `SetNametagColor(Color)` | Установить цвет nametag |
| `ResetNametagColor()` | Сбросить цвет nametag |
| `SetSpectator()` | Перейти в режим наблюдателя |
| `SpectatePlayer(Player)` | Наблюдать за игроком |
| `Unspectate()` | Выйти из наблюдения |
| `SetClothes(int, int, int)` | Установить одежду |
| `GetClothesDrawable(int)` | Получить модель одежды |
| `GetClothesTexture(int)` | Получить текстуру одежды |
| `SetDefaultClothes()` | Сбросить одежду |
| `SetAccessory(int, int, int)` | Установить аксессуар |
| `ClearAccessory(int)` | Убрать аксессуар |
| `GetAccessoryDrawable(int)` | Получить модель аксессуара |
| `GetAccessoryTexture(int)` | Получить текстуру аксессуара |
| `SendMessage(string)` | Отправить сообщение |
| `SendMessage(string, string)` | Отправить цветное сообщение |

---

## См. также

- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
