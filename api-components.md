# NetEase Mod SDK — Component API Reference

## Factory Pattern (Primary API)

Get the engine component factory, then create specific components:

```python
# Server side
factory = serverApi.GetEngineCompFactory()

# Client side
factory = clientApi.GetEngineCompFactory()
```

### All Factory Methods

| Factory Method | Side | Returns | Description |
|---|---|---|---|
| `CreateItem(entityId)` | Server | ItemCompServer | Item/inventory operations |
| `CreateMsg(entityId)` | Server | MsgComponentServer | Send messages to players |
| `CreatePlayer(entityId)` | Server | PlayerCompServer | Player attributes/behavior |
| `CreateGame(levelId)` | Server | GameComponentServer | Game rules, spawning |
| `CreateEntity(entityId)` | Server | EntityCompServer | Entity operations |
| `CreateDimension(levelId)` | Server | DimensionCompServer | Dimension/time operations |
| `CreateWeather(levelId)` | Server | WeatherComponentServer | Weather control |
| `CreateExp(entityId)` | Server | ExpComponentServer | Player experience |
| `CreateLv(entityId)` | Server | LevelComponentServer | Player levels |
| `CreateItemBanned(levelId)` | Server | ItemBannedCompServer | Ban items |
| `CreateEntityDefinitions(entityId)` | Server | EntityDefinitionsCompServer | Entity NBT data |

---

## Item Component (CreateItem / CreateComponent "item")

```python
comp = serverApi.GetEngineCompFactory().CreateItem(entityId)
```

### Server Methods

| Method | Parameters | Returns | Description |
|---|---|---|---|
| `SpawnItemToPlayerInv(itemDict, playerId, slotPos?)` | itemDict(dict), playerId(str), slotPos(int, optional) | bool | Spawn item to player inventory. If slotPos omitted, auto-finds best slot. |
| `SpawnItemToPlayerCarried(itemDict, playerId)` | itemDict(dict), playerId(str) | bool | Spawn item to player's main hand |
| `GetEntityItem(posType, slotPos, getUserData?)` | posType(ItemPosType), slotPos(int), getUserData(bool) | dict/None | Get item info from entity slot |
| `SetEntityItem(posType, slotPos, itemDict)` | posType(ItemPosType), slotPos(int), itemDict(dict) | bool | Set item in entity slot |
| `AddEnchantToInvItem(slotPos, enchantType, level)` | slotPos(int), enchantType(int), level(int) | bool | Add enchant to inventory item |
| `GetEquItemEnchant(slotPos)` | slotPos(ArmorSlotType) | list(tuple)/None | Get enchant list on equipped armor |
| `GetItemDurability(slotPos)` | slotPos(int) | int | Get item durability |
| `SetItemDurability(slotPos, durability)` | slotPos(int), durability(int) | bool | Set item durability |
| `GetItemMaxDurability(slotPos)` | slotPos(int) | int | Get max durability |
| `SetItemMaxDurability(slotPos, maxDurability)` | slotPos(int), maxDurability(int) | bool | Set max durability |
| `SetAttackDamage(damage)` | damage(int) | bool | Set item attack damage |
| `ChangePlayerItemTipsAndExtraId(posType, slotPos, tips, extraId)` | posType(ItemPosType), slotPos(int), tips(str), extraId(str) | bool | Set custom tooltip and identifier |

### Item Information Dictionary

```python
itemDict = {
    "itemName": "minecraft:diamond_sword",  # Namespace:name
    "count": 1,                              # Stack count
    "auxValue": 0,                           # Damage/aux value
    "enchantData": [(enchantType, level)],   # Optional: enchantments
    "customTips": "Custom name",             # Optional: display name
    "extraId": "unique_id",                  # Optional: unique identifier
    "userData": {}                            # Optional: custom data
}
```

---

## Message Component (CreateMsg / CreateComponent "msg")

```python
comp = serverApi.GetEngineCompFactory().CreateMsg(playerId)
```

| Method | Parameters | Returns | Description |
|---|---|---|---|
| `NotifyOneMessage(playerId, msg, color?)` | playerId(str), msg(str), color(str) | None | Send chat message to player with optional color |
| `SendMsg(name, msg)` | name(str), msg(str) | bool | Send message as if from a player to all |
| `SendMsgToPlayer(fromEntityId, toEntityId, msg)` | fromEntityId(str), toEntityId(str), msg(str) | None | Private message between players |

---

## Player Component (CreatePlayer)

```python
comp = serverApi.GetEngineCompFactory().CreatePlayer(playerId)
```

| Method | Parameters | Returns | Description |
|---|---|---|---|
| `SetHealth(health)` | health(int) | bool | Set player health |
| `GetHealth()` | — | int | Get current health |
| `GetMaxHealth()` | — | int | Get max health |
| `SetMaxHealth(maxHealth)` | maxHealth(int) | bool | Set max health |
| `SetHunger(hunger)` | hunger(int) | bool | Set hunger value |
| `GetHunger()` | — | int | Get hunger value |
| `SetSaturation(saturation)` | saturation(float) | bool | Set saturation |
| `GetSaturation()` | — | float | Get saturation |
| `SetGameType(gameType)` | gameType(int) | bool | Set player game mode |
| `GetGameType()` | — | int | Get player game mode |
| `SetName(name)` | name(str) | bool | Set player display name |
| `GetName()` | — | str | Get player display name |
| `IsSneaking()` | — | bool | Check if sneaking |
| `AddPlayerExperience(exp)` | exp(int) | bool | Add experience (use CreateExp for this) |
| `AddPlayerLevel(level)` | level(int) | bool | Add levels (use CreateLv for this) |

---

## Game Component (CreateGame)

```python
comp = serverApi.GetEngineCompFactory().CreateGame(levelId)
# levelId = serverApi.GetLevelId()
```

| Method | Parameters | Returns | Description |
|---|---|---|---|
| `SetTime(time)` | time(int): 0-24000 | bool | Set world time (in frames) |
| `GetTime()` | — | int | Get world time |
| `KillEntity(entityId)` | entityId(str) | bool | Kill an entity |
| `GetEntitiesInSquareArea(pos, axis, halfLength)` | pos(tuple), axis(str), halfLength(int) | list(str) | Get entity IDs in square area |
| `GetEntitiesInSphereArea(pos, radius)` | pos(tuple), radius(int) | list(str) | Get entity IDs in sphere area |

---

## Level/World API (serverApi.*)

Direct functions on `serverApi` (no factory needed):

```python
import mod.server.extraServerApi as serverApi

levelId = serverApi.GetLevelId()
# Use levelId as entityId for world-scoped components
```

---

## Legacy CreateComponent Pattern

Still valid and common in tutorials, but factory pattern is preferred:

```python
# Server-side
comp = serverApi.CreateComponent(entityId, "Minecraft", "item")
comp.SpawnItemToPlayerInv(itemDict, playerId)

msg = serverApi.CreateComponent(serverApi.GetLevelId(), "Minecraft", "msg")
msg.NotifyOneMessage(playerId, "Hello", "§a")

# Client-side
comp = clientApi.CreateComponent(clientApi.GetLocalPlayerId(), "Minecraft", "item")
```

---

## Key Enums

### ItemPosType
```python
# serverApi.GetMinecraftEnum().ItemPosType.xxx
INVENTORY  = 0   # Inventory slots
OFFHAND    = 1   # Offhand
CARRIED    = 2   # Main hand
ARMOR      = 3   # Armor slots
```

### ArmorSlotType
```python
# serverApi.GetMinecraftEnum().ArmorSlotType.xxx
HEAD   = 0  # Helmet
BODY   = 1  # Chestplate
LEG    = 2  # Leggings
FOOT   = 3  # Boots
```

### ActorDamageCause (damage cause IDs)
```python
class ActorDamageCause:
    None_ = -1
    Override = 0
    Contact = 1          # Cactus, etc.
    EntityAttack = 2
    Projectile = 3
    Suffocation = 4
    Fall = 5
    Fire = 6
    FireTick = 7
    Lava = 8
    Drowning = 9
    BlockExplosion = 10
    EntityExplosion = 11
    Void = 12
    Suicide = 13
    Magic = 14
    Wither = 15
    Starve = 16
    Anvil = 17
    Thorns = 18
    FallingBlock = 19
    Piston = 20
    FlyIntoWall = 21
    Magma = 22
    Fireworks = 23
    Lightning = 24
    Freezing = 25
    Stalactite = 26
    Stalagmite = 27
```

### GameType (game modes)
```python
class GameType:
    Survival = 0
    Creative = 1
    Adventure = 2
    Spectator = 3  # (may not be available)
```

---

## System API Reference

### Server APIs (server.extraServerApi)

```python
import mod.server.extraServerApi as serverApi

# System
serverApi.GetServerSystemCls()                                    # Get ServerSystem base class

# Registration (in modMain.py)
serverApi.RegisterSystem(nameSpace, systemName, clsPath)          # Register ServerSystem (string path)

# Engine meta
serverApi.GetEngineNamespace()                                    # "Microsoft"
serverApi.GetEngineSystemName()                                   # "Server" / "Client"

# Level
serverApi.GetLevelId()                                            # Get current level ID

# Component factory
serverApi.GetEngineCompFactory()                                  # Get component factory

# Legacy components
serverApi.CreateComponent(entityId, nameSpace, name)              # Create legacy component
serverApi.GetComponent(entityId, nameSpace, name)                 # Get existing component (None if not created)

# Event
serverApi.GetMinecraftEnum()                                      # Get Minecraft enums (ItemPosType, etc.)
```

### Client APIs (client.extraClientApi)

```python
import mod.client.extraClientApi as clientApi

# System
clientApi.GetClientSystemCls()                                    # Get ClientSystem base class
clientApi.RegisterSystem(nameSpace, systemName, clsPath)          # Register ClientSystem (string path)

# Player
clientApi.GetLocalPlayerId()                                      # Get local player ID

# Engine meta
clientApi.GetEngineNamespace()                                    # "Microsoft"
clientApi.GetEngineSystemName()                                   # "Server" / "Client"

# Component factory
clientApi.GetEngineCompFactory()                                  # Get client component factory
clientApi.CreateComponent(entityId, nameSpace, name)              # Create legacy client component
```

### Mod Binding (mod.common.mod)

```python
from mod.common.mod import Mod

@Mod.Binding(name="ModName", version="1.0.0")
class MyMod(object):
    @Mod.InitServer()
    def serverInit(self): pass

    @Mod.DestroyServer()
    def serverDestroy(self): pass

    @Mod.InitClient()
    def clientInit(self): pass

    @Mod.DestroyClient()
    def clientDestroy(self): pass
```
