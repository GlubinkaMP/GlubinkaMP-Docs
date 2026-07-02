# 👤 Players

Документация по работе с игроками в **Glubinka MP**.

Здесь описаны основные функции API для управления игроками: здоровье, броня, позиция, оружие, скины, сообщения и многое другое.

---

## Объект Client

Каждый подключённый игрок на сервере представлен объектом `Client`.

Ты получаешь этот объект:
```
- в событиях (например `onPlayerConnected`)
- в командах (первый параметр)
- через `API.getAllPlayers()`
- через `API.getPlayerFromName("имя")`
```
---

## Основные свойства игрока

```
string name           = player.Name;
Vector3 position      = player.Position;
Vector3 velocity      = player.Velocity;
int health            = player.Health;
int armor             = player.Armor;
bool inVehicle        = player.IsInVehicle;
NetHandle vehicle     = player.CurrentVehicle;
WeaponHash weapon     = player.CurrentWeapon;
NetHandle handle      = player.handle;
```

---

## Получение списка игроков

### Все игроки на сервере

```
List<Client> allPlayers = API.getAllPlayers();

foreach (Client p in allPlayers)
{
    API.consoleOutput("Игрок: " + p.Name);
}
```

---

### Найти игрока по имени

```
Client target = API.getPlayerFromName("Ivan");

if (target != null)
{
    API.sendChatMessageToPlayer(target, "~g~Тебя нашли!");
}
```

---

### Найти игрока по NetHandle

```
Client player = API.getPlayerFromHandle(someNetHandle);

if (player != null)
{
    API.consoleOutput("Это игрок: " + player.Name);
}
```

---

### Игроки в радиусе от точки

```
List<Client> nearby = API.getPlayersInRadiusOfPosition(50f, new Vector3(0f, 0f, 72f));

foreach (Client p in nearby)
{
    API.sendChatMessageToPlayer(p, "~y~Ты рядом с центром карты.");
}
```

---

### Игроки в радиусе от другого игрока

```
List<Client> nearby = API.getPlayersInRadiusOfPlayer(30f, player);

foreach (Client p in nearby)
{
    API.sendChatMessageToPlayer(p, "~y~Ты рядом с " + player.Name);
}
```

---

## Здоровье

### Получить здоровье

```
int hp = API.getPlayerHealth(player);
API.consoleOutput("HP: " + hp);
```

---

### Установить здоровье

```
API.setPlayerHealth(player, 100);
```

> Значение от 0 до 100. При 0 игрок умирает.

---

### Проверить жив ли игрок

```
bool dead = API.isPlayerDead(player);

if (dead)
{
    API.consoleOutput(player.Name + " мёртв.");
}
```

---

## Броня

### Получить броню

```
int armor = API.getPlayerArmor(player);
API.consoleOutput("Armor: " + armor);
```

---

### Установить броню

```
API.setPlayerArmor(player, 100);
```

---

## Позиция

### Получить позицию

```
Vector3 pos = player.Position;
API.consoleOutput("X: " + pos.X + " Y: " + pos.Y + " Z: " + pos.Z);
```

---

### Телепортировать игрока

```
API.setEntityPosition(player.handle, new Vector3(-269.0f, -955.0f, 31.0f));
```

---

### Получить скорость

```
Vector3 velocity = API.getPlayerVelocity(player);
```

---

### Установить скорость

```
API.setPlayerVelocity(player, new Vector3(0f, 0f, 50f));
```

> Это подбросит игрока вверх.

---

## Заморозка

### Заморозить игрока на месте

```
API.freezePlayer(player, true);
```

---

### Разморозить игрока

```
API.freezePlayer(player, false);
```

---

## Скин (модель персонажа)

### Сменить скин

```
API.setPlayerSkin(player, PedHash.Cop01SFY);
```

Некоторые популярные модели:
```
| PedHash | Описание |
|---|---|
| `PedHash.Michael` | Майкл |
| `PedHash.Franklin` | Франклин |
| `PedHash.Trevor` | Тревор |
| `PedHash.Cop01SFY` | Полицейский |
| `PedHash.Doctor01SMM` | Доктор |
| `PedHash.Fireman01SMY` | Пожарный |
| `PedHash.Paramedic01SMM` | Медик |
| `PedHash.ArmGoon01GMM` | Бандит |
| `PedHash.Business01AFY` | Бизнесвумен |
| `PedHash.FreemodeMale01` | Фримод мужчина |
| `PedHash.FreemodeFemale01` | Фримод женщина |
```
---

### Сбросить одежду на стандартную

```
API.setPlayerDefaultClothes(player);
```

---

## Одежда

### Установить одежду

```
API.setPlayerClothes(player, slot, drawable, texture);
```

Где:
```
| Параметр | Описание |
|---|---|
| `slot` | Часть тела |
| `drawable` | Номер модели одежды |
| `texture` | Номер текстуры |
```
Слоты одежды:
```
| Slot | Часть тела |
|---|---|
| 0 | Лицо |
| 1 | Маска |
| 2 | Причёска |
| 3 | Торс |
| 4 | Ноги |
| 5 | Сумки / парашют |
| 6 | Обувь |
| 7 | Аксессуар |
| 8 | Верхняя одежда |
| 9 | Броня |
| 10 | Наклейки |
| 11 | Верхняя одежда 2 |
```
**Пример:**

```
API.setPlayerClothes(player, 3, 5, 0);   // торс
API.setPlayerClothes(player, 4, 10, 0);  // ноги
API.setPlayerClothes(player, 6, 3, 0);   // обувь
API.setPlayerClothes(player, 8, 15, 0);  // верхняя одежда
API.setPlayerClothes(player, 11, 5, 0);  // верхняя одежда 2
```

---

### Получить текущую одежду

```
int drawable = API.getPlayerClothesDrawable(player, 3);
int texture  = API.getPlayerClothesTexture(player, 3);
```

---

## Аксессуары

### Надеть аксессуар

```
API.setPlayerAccessory(player, slot, drawable, texture);
```

Слоты аксессуаров:
```
| Slot | Описание |
|---|---|
| 0 | Шапка |
| 1 | Очки |
| 2 | Уши |
```
**Пример:**

```
API.setPlayerAccessory(player, 0, 5, 0);  // шапка
API.setPlayerAccessory(player, 1, 3, 0);  // очки
```

---

### Снять аксессуар

```
API.clearPlayerAccessory(player, 0);  // снять шапку
```

---

### Получить текущий аксессуар

```
int drawable = API.getPlayerAccessoryDrawable(player, 0);
int texture  = API.getPlayerAccessoryTexture(player, 0);
```

---

## Оружие

### Выдать оружие

```
API.givePlayerWeapon(player, WeaponHash.Pistol, 100, true, true);
```

Параметры:
```
| Параметр | Тип | Описание |
|---|---|---|
| `player` | `Client` | Игрок |
| `weaponHash` | `WeaponHash` | Хэш оружия |
| `ammo` | `int` | Количество патронов |
| `equipNow` | `bool` | Взять в руки сразу |
| `ammoLoaded` | `bool` | Патроны в обойме |
```
---

### Забрать одно оружие

```
API.removePlayerWeapon(player, WeaponHash.Pistol);
```

---

### Забрать все оружия

```
API.removeAllPlayerWeapons(player);
```

---

### Получить текущее оружие

```
WeaponHash currentWeapon = API.getPlayerCurrentWeapon(player);
```

---

### Получить список оружия

```
WeaponHash[] weapons = API.getPlayerWeapons(player);

foreach (WeaponHash w in weapons)
{
    API.consoleOutput("Оружие: " + w);
}
```

---

### Получить количество патронов

```
int ammo = API.getPlayerWeaponAmmo(player, WeaponHash.Pistol);
```

---

### Установить количество патронов

```
API.setPlayerWeaponAmmo(player, WeaponHash.Pistol, 500);
```

---

### Популярные модели оружия
```
| WeaponHash | Описание |
|---|---|
| `WeaponHash.Pistol` | Пистолет |
| `WeaponHash.CombatPistol` | Боевой пистолет |
| `WeaponHash.SMG` | SMG |
| `WeaponHash.AssaultRifle` | Штурмовая винтовка |
| `WeaponHash.CarbineRifle` | Карбиновая винтовка |
| `WeaponHash.SniperRifle` | Снайперская винтовка |
| `WeaponHash.Shotgun` | Дробовик |
| `WeaponHash.RPG` | РПГ |
| `WeaponHash.Minigun` | Миниган |
| `WeaponHash.Knife` | Нож |
| `WeaponHash.Bat` | Бита |
| `WeaponHash.Grenade` | Граната |
| `WeaponHash.StickyBomb` | Липкая бомба |
| `WeaponHash.SmokeGrenade` | Дымовая граната |
```
---

## Компоненты и скины оружия

### Установить окраску оружия

```
API.setPlayerWeaponTint(player, WeaponHash.Pistol, WeaponTint.Gold);
```

---

### Добавить компонент оружия

```
API.givePlayerWeaponComponent(player, WeaponHash.Pistol, WeaponComponent.AtPiFlsh);
```

---

### Убрать компонент оружия

```
API.removePlayerWeaponComponent(player, WeaponHash.Pistol, WeaponComponent.AtPiFlsh);
```

---

### Получить все доступные компоненты оружия

```
WeaponComponent[] components = API.getAllWeaponComponents(WeaponHash.Pistol);

foreach (WeaponComponent comp in components)
{
    API.consoleOutput("Компонент: " + comp);
}
```

---

## Сообщения игроку

### Сообщение в чат одному игроку

```
API.sendChatMessageToPlayer(player, "~g~Привет!");
```

---

### Сообщение в чат с именем отправителя

```
API.sendChatMessageToPlayer(player, "Сервер", "~y~Добро пожаловать!");
```

---

### Сообщение в чат всем

```
API.sendChatMessageToAll("~r~ВНИМАНИЕ: ~w~Сервер скоро перезапустится.");
```

---

### Сообщение в чат всем с именем отправителя

```
API.sendChatMessageToAll("Админ", "~y~Через 5 минут перезапуск!");
```

---

### Уведомление (notification)

```
API.sendNotificationToPlayer(player, "Ты получил оружие!");
```

---

### Уведомление всем

```
API.sendNotificationToAll("Новый игрок подключился!");
```

---

### Уведомление с картинкой

```
API.sendPictureNotificationToPlayer(
    player,
    "Ты получил новое задание.",
    "CHAR_LESTER",
    0,
    1,
    "Лестер",
    "Новое задание"
);
```

---

### Уведомление с картинкой всем

```
API.sendPictureNotificationToAll(
    "Сервер обновлён!",
    "CHAR_SOCIAL_CLUB",
    0,
    1,
    "Glubinka MP",
    "Обновление"
);
```

---

## Имя игрока

### Получить имя

```
string name = API.getPlayerName(player);
```

---

### Изменить имя

```
bool success = API.setPlayerName(player, "NewName");

if (!success)
{
    API.sendChatMessageToPlayer(player, "~r~Это имя уже занято.");
}
```

---

## Nametag (табличка над головой)

### Установить текст

```
API.setPlayerNametag(player, "~r~[ADMIN] ~w~" + player.Name);
```

---

### Сбросить текст

```
API.resetPlayerNametag(player);
```

---

### Скрыть nametag

```
API.setPlayerNametagVisible(player, false);
```

---

### Показать nametag

```
API.setPlayerNametagVisible(player, true);
```

---

### Изменить цвет

```
API.setPlayerNametagColor(player, 255, 0, 0);  // красный
```

---

### Сбросить цвет

```
API.resetPlayerNametagColor(player);
```

---

## Розыск (Wanted Level)

### Установить уровень розыска

```
API.setPlayerWantedLevel(player, 3);
```

> Значение от 0 (нет розыска) до 5 (максимум).

---

### Получить уровень розыска

```
int wanted = API.getPlayerWantedLevel(player);
```

---

## Команды для игрока

### Команда для управления другим игроком

```
[Command("teleportto")]
public void TeleportToCommand(Client player, string targetName)
{
    Client target = API.getPlayerFromName(targetName);

    if (target == null)
    {
        API.sendChatMessageToPlayer(player, "~r~Игрок не найден.");
        return;
    }

    API.setEntityPosition(player.handle, target.Position);
    API.sendChatMessageToPlayer(player, "~g~Ты телепортирован к " + target.Name);
}
```

---

### Команда для информации об игроке

```
[Command("playerinfo")]
public void PlayerInfoCommand(Client player, string targetName)
{
    Client target = API.getPlayerFromName(targetName);

    if (target == null)
    {
        API.sendChatMessageToPlayer(player, "~r~Игрок не найден.");
        return;
    }

    API.sendChatMessageToPlayer(player, "~y~Имя: ~w~" + target.Name);
    API.sendChatMessageToPlayer(player, "~y~HP: ~w~" + API.getPlayerHealth(target));
    API.sendChatMessageToPlayer(player, "~y~Armor: ~w~" + API.getPlayerArmor(target));
    API.sendChatMessageToPlayer(player, "~y~Ping: ~w~" + API.getPlayerPing(target));
    API.sendChatMessageToPlayer(player, "~y~In Vehicle: ~w~" + target.IsInVehicle);
    API.sendChatMessageToPlayer(player, "~y~Weapon: ~w~" + API.getPlayerCurrentWeapon(target));
}
```

---

## Транспорт и игрок

### Посадить игрока в транспорт

```
API.setPlayerIntoVehicle(player, vehicleHandle, -1);
```

Где seat:
```
| Seat | Описание |
|---|---|
| `-1` | Водитель |
| `0` | Передний пассажир |
| `1` | Задний левый |
| `2` | Задний правый |
```
---

### Высадить игрока из транспорта

```
API.warpPlayerOutOfVehicle(player);
```

---

### Проверить в транспорте ли игрок

```
bool inVehicle = API.isPlayerInAnyVehicle(player);
```

---

### Получить транспорт игрока

```
NetHandle vehicle = API.getPlayerVehicle(player);
```

---

### Получить место в транспорте

```
int seat = API.getPlayerVehicleSeat(player);
```

---

## Пинг и сетевые данные

### Получить пинг

```
int ping = API.getPlayerPing(player);
```

---

### Получить IP адрес

```
string ip = API.getPlayerAddress(player);
```

---

### Проверить подключён ли игрок

```
bool connected = API.isPlayerConnected(player);
```

---

## Состояния игрока

### Стреляет ли игрок

```
bool shooting = API.isPlayerShooting(player);
```

---

### Целится ли игрок

```
bool aiming = API.isPlayerAiming(player);
```

---

### Перезаряжает ли оружие

```
bool reloading = API.isPlayerReloading(player);
```

---

### Горит ли игрок

```
bool onFire = API.isPlayerOnFire(player);
```

---

### В укрытии ли игрок

```
bool inCover = API.isPlayerInCover(player);
```

---

### На лестнице ли игрок

```
bool onLadder = API.isPlayerOnLadder(player);
```

---

### С парашютом ли игрок

```
bool parachuting = API.isPlayerParachuting(player);
```

---

### В свободном падении ли игрок

```
bool freefall = API.isPlayerInFreefall(player);
```

---

### Куда целится игрок

```
Vector3 aimPoint = API.getPlayerAimingPoint(player);
```

---

## Кик и бан

### Кикнуть с причиной

```
API.kickPlayer(player, "Нарушение правил.");
```

---

### Кикнуть без причины

```
API.kickPlayer(player);
```

---

### Забанить с причиной

```
API.banPlayer(player, "Читы.");
```

---

### Забанить без причины

```
API.banPlayer(player);
```

---

### Разбанить по Social Club

```
API.unbanPlayer("socialClubName");
```

---

## Наблюдение (Spectator)

### Включить режим наблюдателя

```
API.setPlayerToSpectator(player);
```

---

### Наблюдать за другим игроком

```
API.setPlayerToSpectatePlayer(player, targetPlayer);
```

---

### Выйти из режима наблюдателя

```
API.unspectatePlayer(player);
```

---

### Проверить наблюдает ли игрок

```
bool spectating = API.isPlayerSpectating(player);
```

---

## Анимации

### Проигрывать анимацию

```
API.playPlayerAnimation(player, 0, "amb@world_human_cheering@male_a", "base");
```

Параметры:
```
| Параметр | Описание |
|---|---|
| `player` | Игрок |
| `flag` | Флаг анимации (0 = обычная) |
| `animDict` | Словарь анимации |
| `animName` | Имя анимации |
```
---

### Проигрывать сценарий

```
API.playPlayerScenario(player, "WORLD_HUMAN_SMOKING");
```

---

### Остановить анимацию

```
API.stopPlayerAnimation(player);
```

---

### Очистить все задания

```
API.clearPlayerTasks(player);
```

---

## Ремень безопасности

### Пристегнуть

```
API.setPlayerSeatbelt(player, true);
```

---

### Отстегнуть

```
API.setPlayerSeatbelt(player, false);
```

---

### Проверить

```
bool seatbelt = API.getPlayerSeatbelt(player);
```

---

## Команды

### Команда для выдачи оружия

```
[Command("givegun")]
public void GiveGunCommand(Client player, string weaponName, int ammo)
{
    WeaponHash weapon = API.weaponNameToModel(weaponName);

    if ((int)weapon == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестное оружие.");
        return;
    }

    if (ammo < 1) ammo = 1;
    if (ammo > 9999) ammo = 9999;

    API.givePlayerWeapon(player, weapon, ammo, true, true);
    API.sendChatMessageToPlayer(player, "~g~Выдано: ~w~" + weaponName + " x" + ammo);
}
```

---

### Команда для смены скина

```
[Command("skin")]
public void SkinCommand(Client player, string modelName)
{
    PedHash model = API.pedNameToModel(modelName);

    if ((int)model == 0)
    {
        API.sendChatMessageToPlayer(player, "~r~Неизвестная модель.");
        return;
    }

    API.setPlayerSkin(player, model);
    API.sendChatMessageToPlayer(player, "~g~Скин изменён на: ~w~" + modelName);
}
```

---

## Частые ошибки

### Ошибка 1 — не проверил null при поиске игрока

Неправильно:
```
Client target = API.getPlayerFromName(name);
API.setPlayerHealth(target, 100);
```

Правильно:
```
Client target = API.getPlayerFromName(name);
if (target == null)
{
    API.sendChatMessageToPlayer(player, "~r~Игрок не найден.");
    return;
}
API.setPlayerHealth(target, 100);
```

---

### Ошибка 2 — забыл что здоровье 0 = смерть

```
API.setPlayerHealth(player, 0);
```

> Это убьёт игрока. Если хочешь "почти мёртвого" — ставь 1.

---

### Ошибка 3 — забыл проверить модель

```
PedHash model = API.pedNameToModel(name);
API.setPlayerSkin(player, model);
```

> Если `name` неправильный, `model` будет равен 0, и это может вызвать проблемы.

---

## Цветовые коды для чата
```
| Код | Цвет |
|---|---|
| `~r~` | Красный |
| `~g~` | Зелёный |
| `~b~` | Синий |
| `~y~` | Жёлтый |
| `~w~` | Белый |
| `~p~` | Фиолетовый |
| `~o~` | Оранжевый |
```
---

## См. также

- [Events](https://glubinkamp.github.io/GlubinkaMP-Docs/Events.html)
- [Commands](https://glubinkamp.github.io/GlubinkaMP-Docs/Commands.html)
- [Vehicles](https://glubinkamp.github.io/GlubinkaMP-Docs/Vehicles.html)
- [World](https://glubinkamp.github.io/GlubinkaMP-Docs/World.html)
- [CEF UI](https://glubinkamp.github.io/GlubinkaMP-Docs/CEF_UI.html)
- [FAQ](https://glubinkamp.github.io/GlubinkaMP-Docs/FAQ.html)
```
