# Player

Документация по работе с игроками в **Glubinka MP SDK**.

Этот файл описывает публичный SDK-класс `Player` из пространства имён `GlubinkaMP.SDK`.

---

## Что такое Player

`Player` — это публичная обёртка над внутренним объектом игрока.

Она нужна для того, чтобы разработчику ресурса не приходилось работать с низкоуровневыми методами ядра напрямую. Вместо этого ты получаешь удобный объект с понятными свойствами и методами.

---

## Подключение namespaces

```csharp
using GlubinkaMP.SDK;
using GTANetworkServer;
using GTANetworkShared;
```

---

## Как получить Player

В командах и событиях ядро передаёт объект `Client`.
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
string name        = player.Name;
string socialClub  = player.SocialClubName;
Vector3 position   = player.Position;
Vector3 rotation   = player.Rotation;
Vector3 velocity   = player.Velocity;
int health         = player.Health;
int armor          = player.Armor;
int ping           = player.Ping;
bool inVehicle     = player.IsInVehicle;
int currentWeapon  = player.CurrentWeapon;
int modelHash      = player.ModelHash;
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
bool connected   = player.IsConnected;
bool dead        = player.IsDead;
bool respawning  = player.IsRespawning;
bool onFire      = player.IsOnFire;
bool parachuting = player.IsParachuting;
bool freefall    = player.IsInFreefall;
bool aiming      = player.IsAiming;
bool shooting    = player.IsShooting;
bool reloading   = player.IsReloading;
bool inCover     = player.IsInCover;
bool onLadder    = player.IsOnLadder;
bool spectating  = player.IsSpectating;
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
Vector3 aimPos    = player.AimPosition;
string address    = player.Address;
int team          = player.Team;
int wanted        = player.WantedLevel;
int seat          = player.VehicleSeat;
NetHandle vehicle = player.CurrentVehicle;
bool seatbelt     = player.Seatbelt;
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
string tag    = player.Nametag;
bool visible  = player.NametagVisible;
Color color   = player.NametagColor;
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
player.ShowNametag();             // alias для SetNametagVisible(true)
player.HideNametag();             // alias для SetNametagVisible(false)
player.SetNametagColor(new Color(255, 0, 0));
player.ResetNametagColor();
```

| Метод | Описание |
|---|---|
| `SetNametag(string)` | Установить текст nametag |
| `ResetNametag()` | Сбросить к стандартному |
| `SetNametagVisible(bool)` | Показать или скрыть |
| `ShowNametag()` | Показать nametag |
| `HideNametag()` | Скрыть nametag |
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
player.SetVelocity(new Vector3(0, 0, 50.0f));  // подбросить вверх
player.SetVelocity(0, 0, 50.0f);               // то же самое
```

### Заморозить / разморозить

```csharp
player.Freeze();         // заморозить
player.Freeze(true);     // заморозить
player.Freeze(false);    // разморозить
player.Unfreeze();       // alias для Freeze(false)
```

| Метод | Описание |
|---|---|
| `Freeze()` | Заморозить игрока |
| `Freeze(bool)` | Заморозить или разморозить |
| `Unfreeze()` | Разморозить игрока |

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

Одежда устанавливается через номер слота или через удобный enum `ClothesSlot`.

### Установить одежду (через число)

```csharp
player.SetClothes(11, 15, 0);
```

### Установить одежду (через enum)

```csharp
player.SetClothes(ClothesSlot.Tops, 15, 0);
```

### Прочитать одежду

```csharp
int drawable = player.GetClothesDrawable(ClothesSlot.Tops);
int texture  = player.GetClothesTexture(ClothesSlot.Tops);
```

### Сбросить к стандартной

```csharp
player.SetDefaultClothes();
```

### Слоты одежды (`ClothesSlot`)

| Значение enum | Число | Описание |
|---|---|---|
| `ClothesSlot.Head` | 0 | Голова |
| `ClothesSlot.Mask` | 1 | Маска / лицо |
| `ClothesSlot.Torso` | 3 | Верхняя одежда (куртки, пальто) |
| `ClothesSlot.Legs` | 4 | Штаны / низ |
| `ClothesSlot.Bags` | 5 | Сумки / парашют |
| `ClothesSlot.Shoes` | 6 | Обувь |
| `ClothesSlot.Accessories` | 7 | Шарфы / цепи (аксессуары шеи) |
| `ClothesSlot.Undershirt` | 8 | Рубашки / футболки |
| `ClothesSlot.BodyArmor` | 9 | Бронежилеты |
| `ClothesSlot.Decals` | 10 | Значки / декали |
| `ClothesSlot.Tops` | 11 | Джемперы / рубашки поверх |

### Пример

```csharp
[Command("outfit")]
public void Outfit(Client client)
{
    var player = WrapPlayer(client);
    player.SetClothes(ClothesSlot.Tops, 15, 0);
    player.SetClothes(ClothesSlot.Legs, 10, 0);
    player.SetClothes(ClothesSlot.Shoes, 3, 0);
    player.SendMessage("~g~Одежда надета.");
}
```

---

## Аксессуары

Аксессуары — это отдельные предметы поверх модели (шляпы, очки, серьги и т.п.).
Устанавливаются через число или через удобный enum `AccessorySlot`.

### Установить аксессуар (через enum)

```csharp
player.SetAccessory(AccessorySlot.Hat, 45, 0);
player.SetAccessory(AccessorySlot.Glasses, 5, 0);
```

### Убрать аксессуар

```csharp
player.ClearAccessory(AccessorySlot.Hat);
```

### Прочитать аксессуар

```csharp
int drawable = player.GetAccessoryDrawable(AccessorySlot.Hat);
int texture  = player.GetAccessoryTexture(AccessorySlot.Hat);
```

### Слоты аксессуаров (`AccessorySlot`)

| Значение enum | Число | Описание |
|---|---|---|
| `AccessorySlot.Hat` | 0 | Головные уборы (шляпы, кепки) |
| `AccessorySlot.Glasses` | 1 | Очки |
| `AccessorySlot.Ears` | 2 | Уши (серьги, наушники) |
| `AccessorySlot.Watch` | 6 | Часы |
| `AccessorySlot.Bracelet` | 7 | Браслеты |

### Пример

```csharp
[Command("glasses")]
public void Glasses(Client client)
{
    var player = WrapPlayer(client);
    player.SetAccessory(AccessorySlot.Glasses, 5, 0);
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
player.StartSpectating(targetPlayer);  // alias
```

### Выйти из режима наблюдения

```csharp
player.Unspectate();
player.StopSpectating();  // alias
```

| Метод | Описание |
|---|---|
| `SetSpectator()` | Перейти в режим свободной камеры |
| `SpectatePlayer(Player)` | Наблюдать за игроком |
| `StartSpectating(Player)` | Alias для SpectatePlayer |
| `Unspectate()` | Выйти из наблюдения |
| `StopSpectating()` | Alias для Unspectate |

### Пример

```csharp
[Command("spec")]
public void Spec(Client client, string targetName)
{
    var player = WrapPlayer(client);
    Client targetClient = API.getPlayerFromName(targetName);

    if (targetClient == null)
    {
        player.SendError("Игрок не найден.");
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
player.SetSeatbelt(true);      // пристегнуть
player.SetSeatbelt(false);     // отстегнуть
player.EnableSeatbelt();       // alias для SetSeatbelt(true)
player.DisableSeatbelt();      // alias для SetSeatbelt(false)
```

### Проверить

```csharp
bool buckled = player.Seatbelt;
```

| Метод | Описание |
|---|---|
| `SetSeatbelt(bool)` | Пристегнуть или отстегнуть |
| `EnableSeatbelt()` | Пристегнуть |
| `DisableSeatbelt()` | Отстегнуть |

---

## Имя игрока

### Сменить имя

```csharp
player.SetName("NewNickname");
```

> Имя не может быть пустым или состоять из пробелов.

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
        player.SendError("Игрок не найден.");
        return;
    }

    var target = WrapPlayer(targetClient);
    target.Kick(reason);
}
```

---

## Работа с транспортом

### Посадить игрока в транспорт (через число)

```csharp
player.PutIntoVehicle(vehicle);      // водитель
player.PutIntoVehicle(vehicle, 0);   // передний пассажир
```

### Посадить игрока в транспорт (через enum)

```csharp
player.PutIntoVehicle(vehicle, VehicleSeat.Driver);
player.PutIntoVehicle(vehicle, VehicleSeat.Passenger);
```

### Значения сидений

| VehicleSeat | Число | Описание |
|---|---|---|
| `VehicleSeat.Driver` | `-1` | Водитель |
| `VehicleSeat.Passenger` | `0` | Передний пассажир |
| `VehicleSeat.LeftRear` | `1` | Заднее левое |
| `VehicleSeat.RightRear` | `2` | Заднее правое |

### Высадить игрока из транспорта

```csharp
player.RemoveFromVehicle();
player.WarpOutOfVehicle();   // alias, делает то же самое
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

### Готовые цветные методы

```csharp
player.SendSuccess("Операция выполнена.");   // зелёный
player.SendError("Что-то пошло не так.");    // красный
player.SendWarning("Будь осторожен.");       // жёлтый
player.SendInfo("Информация.");              // синий
```

| Метод | Цвет | Описание |
|---|---|---|
| `SendMessage(string)` | Белый | Обычное сообщение |
| `SendMessage(string, string)` | Любой | Цветное сообщение |
| `SendSuccess(string)` | Зелёный `~g~` | Успех |
| `SendError(string)` | Красный `~r~` | Ошибка |
| `SendWarning(string)` | Жёлтый `~y~` | Предупреждение |
| `SendInfo(string)` | Синий `~b~` | Информация |

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
        player.SetSkin(PedHash.FreemodeMale01);
        player.GiveWeapon(WeaponHash.Pistol, 90);
        player.GiveWeapon(WeaponHash.AssaultRifle, 200);
        player.SetWeaponTint(WeaponHash.Pistol, WeaponTint.Gold);
        player.SetClothes(ClothesSlot.Tops, 15, 0);
        player.SetAccessory(AccessorySlot.Hat, 45, 0);
        player.SendSuccess("Ты получил стартовый набор.");
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
        player.SendInfo("Ты заморожен.");
    }

    [Command("unfreeze")]
    public void UnfreezeMe(Client client)
    {
        var player = WrapPlayer(client);
        player.Unfreeze();
        player.SendSuccess("Ты разморожен.");
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

> Допустимо от `0` до `200`. При выходе за диапазон SDK выбросит ошибку.

---

### Ошибка 3 — работаешь с Vehicle, но передал null

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

### Ошибка 7 — передал null в SpectatePlayer

Неправильно:

```csharp
player.SpectatePlayer(null);
```

Правильно:

```csharp
if (target != null)
    player.SpectatePlayer(target);
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
| `Unfreeze()` | Разморозить |
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
| `GiveWeaponComponent(WeaponHash, WeaponComponent)` | Добавить компонент |
| `RemoveWeaponComponent(WeaponHash, WeaponComponent)` | Убрать компонент |
| `HasWeaponComponent(WeaponHash, WeaponComponent)` | Проверить компонент |
| `GetWeaponComponents(WeaponHash)` | Получить все компоненты |
| `DetonateStickies()` | Взорвать липкие бомбы |
| `PutIntoVehicle(Vehicle, int)` | Посадить в транспорт |
| `PutIntoVehicle(Vehicle, VehicleSeat)` | Посадить в транспорт через enum |
| `RemoveFromVehicle()` | Высадить из транспорта |
| `WarpOutOfVehicle()` | Высадить из транспорта (alias) |
| `SetTeam(int)` | Установить команду |
| `SetWantedLevel(int)` | Установить розыск (0–5) |
| `SetSeatbelt(bool)` | Пристегнуть / отстегнуть ремень |
| `EnableSeatbelt()` | Пристегнуть ремень |
| `DisableSeatbelt()` | Отстегнуть ремень |
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
| `ShowNametag()` | Показать nametag |
| `HideNametag()` | Скрыть nametag |
| `SetNametagColor(Color)` | Установить цвет nametag |
| `ResetNametagColor()` | Сбросить цвет nametag |
| `SetSpectator()` | Перейти в режим наблюдателя |
| `SpectatePlayer(Player)` | Наблюдать за игроком |
| `StartSpectating(Player)` | Alias для SpectatePlayer |
| `Unspectate()` | Выйти из наблюдения |
| `StopSpectating()` | Alias для Unspectate |
| `SetClothes(int, int, int)` | Установить одежду |
| `SetClothes(ClothesSlot, int, int)` | Установить одежду через enum |
| `GetClothesDrawable(int)` | Получить модель одежды |
| `GetClothesDrawable(ClothesSlot)` | Получить модель одежды через enum |
| `GetClothesTexture(int)` | Получить текстуру одежды |
| `GetClothesTexture(ClothesSlot)` | Получить текстуру одежды через enum |
| `SetDefaultClothes()` | Сбросить одежду |
| `SetAccessory(int, int, int)` | Установить аксессуар |
| `SetAccessory(AccessorySlot, int, int)` | Установить аксессуар через enum |
| `ClearAccessory(int)` | Убрать аксессуар |
| `ClearAccessory(AccessorySlot)` | Убрать аксессуар через enum |
| `GetAccessoryDrawable(int)` | Получить модель аксессуара |
| `GetAccessoryDrawable(AccessorySlot)` | Получить модель аксессуара через enum |
| `GetAccessoryTexture(int)` | Получить текстуру аксессуара |
| `GetAccessoryTexture(AccessorySlot)` | Получить текстуру аксессуара через enum |
| `SendMessage(string)` | Отправить сообщение |
| `SendMessage(string, string)` | Отправить цветное сообщение |
| `SendSuccess(string)` | Зелёное сообщение |
| `SendError(string)` | Красное сообщение |
| `SendWarning(string)` | Жёлтое сообщение |
| `SendInfo(string)` | Синее сообщение |

---

## См. также
