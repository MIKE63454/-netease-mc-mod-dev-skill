---
name: netease-mc-mod-dev
description: Use when developing Minecraft NetEase (网易我的世界) mods with Mod SDK, creating behavior packs with Python scripts, writing modMain.py, ServerSystem/ClientSystem classes, registering events/components, or debugging NetEase mod code. Covers mod structure, API patterns, and conventions specific to the NetEase Minecraft edition.
---

# NetEase Minecraft Mod Development

## Overview

NetEase Minecraft mods use **Mod SDK** — a Python-based framework that wraps game events and engine components. Mods are packaged as **AddOns** (behavior packs + resource packs) and developed in **MC Studio**.

Client-Server architecture: **server** handles logic/data, **client** handles rendering/UI. Scripts for each side are isolated — use **custom events** for cross-side communication, never cross-import.

**API Reference Files:** When you need specific event parameters, component method signatures, or enum values, load the reference files:
- `api-events.md` — All engine events with trigger conditions and parameters
- `api-components.md` — All component factory methods, signatures, key enums, and system APIs

**Cold API Fallback:** When encountering an API, event, or concept NOT covered in the reference files above, fetch the full documentation from the GitHub repository. The complete developer docs (1,448 pages from mc.163.com) are in the `我的世界开发者文档/` directory of the repo at `https://github.com/MIKE63454/-netease-mc-mod-dev-skill`. Use `WebFetch` to read specific `.md` files from the raw content URL pattern:
```
https://raw.githubusercontent.com/MIKE63454/-netease-mc-mod-dev-skill/main/%E6%88%91%E7%9A%84%E4%B8%96%E7%95%8C%E5%BC%80%E5%8F%91%E8%80%85%E6%96%87%E6%A1%A3/[path].md
```
(URL-encoded: `我的世界开发者文档` → `%E6%88%91%E7%9A%84%E4%B8%96%E7%95%8C%E5%BC%80%E5%8F%91%E8%80%85%E6%96%87%E6%A1%A3`)
To locate the right file, check `_all_index.json` at the repo root for the full file listing. Common doc paths:
- `mcdocs/1-ModAPI/事件/` — Event reference by category (世界, 玩家, 实体, 方块, 物品, 音效, UI, etc.)
- `mcdocs/1-ModAPI/接口/` — Component API by domain (世界, 玩家, 实体, 方块, 特效, 自定义UI, etc.)
- `mcdocs/1-ModAPI/枚举值/` — Enum definitions (EnchantType, ItemPosType, etc.)
- `mcguide/20-玩法开发/13-模组SDK编程/` — Mod SDK programming tutorials
- `mcguide/20-玩法开发/15-自定义游戏内容/` — Custom game content (items, blocks, entities, etc.)

## Quick Reference

### Mod File Structure

```
behavior_pack_[ModName]/
  manifest.json                        # Mod metadata — 2 UUIDs required
  pack_icon.png                        # 64×64 icon
  [TeamName][ModName]Scripts/          # Scripts root — name must be unique
    __init__.py                        # Empty file, Python module marker (REQUIRED)
    modMain.py                         # Entry point (REQUIRED, must be at root of Scripts/)
    [modName]ServerSystem.py           # Server logic — game rules, items, entities
    [modName]ClientSystem.py           # Client logic — UI, effects, rendering
    [optional].py                      # Any additional modules
resource_pack_[ModName]/
  manifest.json                        # Resource pack manifest — different UUIDs
  textures/                            # Block textures, UI assets
  ...
```

### modMain.py Template

```python
# -*- coding: utf-8 -*-
from mod.common.mod import Mod

@Mod.Binding(name="MyMod", version="1.0.0")
class MyMod(object):

    def __init__(self):
        pass

    @Mod.InitServer()
    def serverInit(self):
        serverApi.RegisterSystem(
            "MyMod", "MyServerSystem",
            "MyModScripts.myServerSystem.MyServerSystem"  # STRING PATH, not instance
        )

    @Mod.DestroyServer()
    def serverDestroy(self):
        pass

    @Mod.InitClient()
    def clientInit(self):
        clientApi.RegisterSystem(
            "MyMod", "MyClientSystem",
            "MyModScripts.myClientSystem.MyClientSystem"
        )

    @Mod.DestroyClient()
    def clientDestroy(self):
        pass
```

### ServerSystem Template

```python
# -*- coding: utf-8 -*-
import server.extraServerApi as serverApi
from mod_log import logger

ServerSystem = serverApi.GetServerSystemCls()

class MyServerSystem(ServerSystem):

    def __init__(self, namespace, systemName):
        ServerSystem.__init__(self, namespace, systemName)
        logger.info("[MyMod] ServerSystem initialized")
        # CRITICAL: 引擎不会自动调用 ListenEvent，必须手动调用
        self.ListenEvent()

    def ListenEvent(self):
        self.ListenForEvent(
            serverApi.GetEngineNamespace(), serverApi.GetEngineSystemName(),
            "AddServerPlayerEvent",
            self, self.OnPlayerJoin
        )
        self.ListenForEvent(
            serverApi.GetEngineNamespace(), serverApi.GetEngineSystemName(),
            "ServerChatEvent",
            self, self.OnChat
        )

    def OnPlayerJoin(self, args):
        playerId = args["id"]
        # Give item using factory pattern:
        comp = serverApi.GetEngineCompFactory().CreateItem(playerId)
        comp.SpawnItemToPlayerInv(
            {"newItemName": "minecraft:diamond_sword", "count": 1, "newAuxValue": 0},
            playerId
        )
        # Send message:
        msg = serverApi.GetEngineCompFactory().CreateMsg(playerId)
        msg.NotifyOneMessage(playerId, "§aWelcome!", "§e")

    def OnChat(self, args):
        message = args.get("message", "").strip()
        playerId = args.get("playerId", "")
        if message == "/hello":
            msg = serverApi.GetEngineCompFactory().CreateMsg(playerId)
            msg.NotifyOneMessage(playerId, "§eHello from MyMod!", "§e")
```

### ClientSystem Template

```python
# -*- coding: utf-8 -*-
import client.extraClientApi as clientApi
from mod_log import logger

ClientSystem = clientApi.GetClientSystemCls()

class MyClientSystem(ClientSystem):

    def __init__(self, namespace, systemName):
        ClientSystem.__init__(self, namespace, systemName)
        logger.info("[MyMod] ClientSystem initialized")
        # CRITICAL: 引擎不会自动调用 ListenEvent，必须手动调用
        self.ListenEvent()

    def ListenEvent(self):
        self.ListenForEvent(
            clientApi.GetEngineNamespace(), clientApi.GetEngineSystemName(),
            "OnLocalPlayerStopLoading",
            self, self.OnPlayerReady
        )

    def OnPlayerReady(self, args):
        logger.info("[MyMod] Client: player ready id=%s", args.get("id"))
```

## Component API Patterns

### Factory Pattern (Preferred — Official API Docs)

```python
# Item operations
comp = serverApi.GetEngineCompFactory().CreateItem(entityId)
comp.SpawnItemToPlayerInv(itemDict, playerId)

# Messaging
comp = serverApi.GetEngineCompFactory().CreateMsg(playerId)
comp.NotifyOneMessage(playerId, "message text", "§a")

# Game operations
comp = serverApi.GetEngineCompFactory().CreateGame(levelId)
comp.SetTime(18000)  # Set to midnight
```

### Legacy Pattern (Common in Tutorial Docs)

```python
# Also valid but older style:
comp = serverApi.CreateComponent(serverApi.GetLevelId(), "Minecraft", "item")
comp.SpawnItemToPlayerInv(itemDict, playerId)

msg = serverApi.CreateComponent(serverApi.GetLevelId(), "Minecraft", "msg")
msg.NotifyOneMessage(playerId, "message", "§e")
```

## Common Patterns

### Server ↔ Client Communication (Custom Events)

Server and client cannot import each other's code. Use custom events for cross-side communication.

**Server → Client:**
```python
# Server side: send data to a specific player
self.NotifyToClient(playerId, "MyMod_ShowTip", {"msg": "Boss defeated!", "color": "§c"})

# Client side: listen for the custom event
def ListenEvent(self):
    self.ListenForEvent("MyMod", "MyClientSystem",
        "MyMod_ShowTip",
        self, self.OnShowTip)

def OnShowTip(self, args):
    msg = args["msg"]
    color = args["color"]
    # Show UI tip, play sound, etc.
```

**Client → Server:**
```python
# Client side: send request to server
self.NotifyToServer("MyMod_RequestItem", {"item": "diamond_sword"})

# Server side: listen for the custom event
def ListenEvent(self):
    self.ListenForEvent("MyMod", "MyServerSystem",
        "MyMod_RequestItem",
        self, self.OnRequestItem)

def OnRequestItem(self, args):
    playerId = args.get("__id__")  # Auto-injected by engine
    itemName = args["item"]
    # Handle the request
```

### Tick Update Loop

For per-frame logic (movement, timers, state machines):

```python
def ListenEvent(self):
    self.ListenForEvent(
        serverApi.GetEngineNamespace(), serverApi.GetEngineSystemName(),
        "OnScriptTickServer",
        self, self.OnTick
    )

def OnTick(self, args=None):
    # CRITICAL: args=None (not args) — engine calls tick with 0 arguments
    # Called 30 times per second
    # Keep logic lightweight — heavy ops cause lag
    pass
```

> **IMPORTANT:** `OnScriptTickServer` fires with **no arguments**. The callback signature MUST be `def OnTick(self, args=None)` — otherwise you get `TypeError: OnTick() takes exactly 2 arguments (1 given)` every tick.

### Data Persistence (Save/Load)

Use `extraData` component to persist data across game sessions:

```python
# Save data
comp = serverApi.GetEngineCompFactory().CreateExtraData(entityId)
comp.SetExtraData("myKey", value)          # Simple key-value
comp.SetExtraData("myDict", {"a": 1})      # Dict OK
comp.SaveExtraData()                        # Commit to disk

# Load data
comp = serverApi.GetEngineCompFactory().CreateExtraData(entityId)
value = comp.GetExtraData("myKey")          # Returns value or None
allData = comp.GetAllExtraData()            # Returns dict of all saved keys
```

> `entityId` for player-specific data: use `playerId`. For world-level data: use `levelId`.

### Custom Items / Blocks (Python Side)

Custom items/blocks defined in JSON still need Python for interactive behavior:

```python
# Custom block interaction
def ListenEvent(self):
    self.ListenForEvent(
        serverApi.GetEngineNamespace(), serverApi.GetEngineSystemName(),
        "ServerBlockUseEvent",
        self, self.OnBlockUse
    )

def OnBlockUse(self, args):
    blockName = args["blockName"]
    playerId = args["playerId"]
    # Check against your custom block's identifier
    if blockName == "mymod:custom_block":
        comp = serverApi.GetEngineCompFactory().CreateItem(playerId)
        comp.SpawnItemToPlayerInv(
            {"itemName": "minecraft:diamond", "count": 1, "auxValue": 0},
            playerId
        )
```

```python
# Custom item usage detection (via chat or other trigger)
# Custom items use namespace:name identifiers
itemDict = {"itemName": "mymod:custom_sword", "count": 1, "auxValue": 0}
```

### Player Inventory Access (CRITICAL)

**`GetPlayerAllItems` and `GetPlayerItem` are the CORRECT APIs for player inventories.** Do NOT use `GetEntityItem` for player inventory slots — it only works for main hand (CARRIED).

```python
itemComp = serverApi.GetEngineCompFactory().CreateItem(playerId)
ItemPosType = serverApi.GetMinecraftEnum().ItemPosType

# 获取整个背包（36格，空槽返回 None）
invItems = itemComp.GetPlayerAllItems(ItemPosType.INVENTORY, False)
if invItems:
    for itemDict in invItems:
        if itemDict:  # 空槽为 None
            name = itemDict.get("newItemName", "") or itemDict.get("itemName", "")
            count = itemDict.get("count", 1)

# 获取单个槽位
offhand = itemComp.GetPlayerItem(ItemPosType.OFFHAND, 0, False)
armor = itemComp.GetPlayerItem(ItemPosType.ARMOR, 2, False)  # 护腿

# 获取主手（这个用 GetEntityItem 也可以）
carried = itemComp.GetEntityItem(ItemPosType.CARRIED, 0, False)
```

> `GetEntityItem` with `ItemPosType.INVENTORY` returns `None` and triggers `[WARNING] get pos type wrong!`. It's designed for non-player entities (donkeys, zombies, etc.). Use `GetPlayerAllItems`/`GetPlayerItem` for players.

### PlayerOperationComponent (Movement Detection)

Detect sprinting, swimming, jumping states via the legacy component pattern:

```python
# Legacy pattern — no factory equivalent for this component
opComp = serverApi.CreateComponent(playerId, "Minecraft", "playerOperation")
if opComp:
    if opComp.IsSprinting():    # 疾跑
        pass
    elif opComp.IsSwimming():   # 游泳
        pass
    elif opComp.IsMoving():     # 行走（站立不动为 False）
        pass
    if opComp.IsJumping():      # 跳跃（上升沿检测避免重复触发）
        pass
```

Available methods: `IsSprinting()`, `IsSwimming()`, `IsMoving()`, `IsJumping()`, `IsSneaking()`, `IsInWater()`, `IsInLava()`, `IsFlying()`, `IsOnGround()`, `IsSleeping()`, `IsRiding()`, `IsUsingItem()`, `IsOnLadder()`.

### modMain.py Path (CRITICAL)

**`__file__` does NOT work in the NetEase engine** — it returns an internal version string like `"3.7.0.291651"` instead of the file path. The scripts folder name MUST be hardcoded:

```python
# -*- coding: utf-8 -*-
from mod.common.mod import Mod

# WRONG: _MOD_DIR = os.path.basename(os.path.dirname(os.path.abspath(__file__)))
# CORRECT: scripts folder name hardcoded
_MOD_DIR = "FzWeightModScripts"

@Mod.Binding(name="WeightMod", version="1.0.0")
class WeightMod(object):
    @Mod.InitServer()
    def serverInit(self):
        import server.extraServerApi as serverApi
        serverApi.RegisterSystem(
            "WeightMod", "WeightServerSystem",
            "{}.weightServerSystem.WeightServerSystem".format(_MOD_DIR)
        )
    # ...
```

### § Color Codes — Encoding Issues

Using `\xa7` in source code triggers `SyntaxError: 'utf8' codec can't decode byte 0xa7`. Use one of:

```python
# Option 1: unicode section sign directly (valid UTF-8, may still garble in some contexts)
S = u"§"

# Option 2: unichr at runtime (safest)
S = unichr(167)

# Option 3 (recommended): skip color codes in commands/HUD, use plain text
```

Color codes work in `NotifyOneMessage` but may garble `/title actionbar` and `/title subtitle`.

## Common Recipes

> Map user requests to the right approach. Detail APIs in `[[api-events]]` and `[[api-components]]`.

| User wants to... | Listen to | Use component |
|---|---|---|
| Welcome message + starter item | `AddServerPlayerEvent` | `CreateMsg`, `CreateItem` |
| Chat commands (/give, /heal) | `ServerChatEvent` | `CreateItem`, `CreatePlayer`, `CreateCommand` |
| Custom block interaction | `ServerBlockUseEvent` | `CreateItem`, `CreateMsg` |
| Custom item effect on use | `ActorUseItemServerEvent` | `CreateHurt`, `CreateEffect` |
| Prevent block break/place | `ServerPlayerTryDestroyBlockEvent` / `ServerEntityTryPlaceBlockEvent` | (set `cancel=True` in args) |
| Boss fight / custom damage | `OnScriptTickServer` + `PlayerHurtEvent` | `CreateHurt`, `CreateEntityEvent` |
| Scoreboard / minigame stats | `OnScriptTickServer` + `PlayerDieEvent` | `CreateCommand` (SetCommand for /scoreboard) |
| Data that survives re-login | `PlayerRespawnFinishServerEvent` | `CreateExtraData` (save on quit, load on join) |
| Show UI to player | `OnLocalPlayerStopLoading` | `NotifyToClient` → client creates UI |
| Server-wide broadcast | `AddServerPlayerEvent` | `CreateMsg` + iterate players |
| Scan player inventory weight | `OnScriptTickServer` | `CreateItem` → `GetPlayerAllItems(INVENTORY)` + `GetPlayerItem(OFFHAND/ARMOR)` |
| Detect sprint/swim/jump | `OnScriptTickServer` | `CreateComponent("Minecraft", "playerOperation")` → `IsSprinting()` etc. |
| Apply speed/slowness | `OnScriptTickServer` | `CreateEffect` → `AddEffectToEntity("slowness", duration, amplifier, False)` |
| HUD via actionbar | `OnScriptTickServer` | `CreateCommand` → `SetCommand("/title @s actionbar ...")` |
| Play sound to player | (on event) | `CreateCommand` → `SetCommand("/playsound <sound> @s")` |
| Persistent stamina/level | `AddServerPlayerEvent` + `OnPlayerLeave` | `CreateExtraData` → `SetExtraData`/`GetExtraData`/`SaveExtraData` |
| Food buff detection | `PlayerEatFoodServerEvent` | `itemDict` → `get("newItemName")` to check food type |
| Jump height control | `OnScriptTickServer` | `CreateCommand` → `SetCommand("/attribute @s minecraft:generic.jump_strength base set <value>")` |

### Broadcast to All Players

```python
# Simple broadcast (simulates a player saying something)
msgComp = serverApi.GetEngineCompFactory().CreateMsg(serverApi.GetLevelId())
msgComp.SendMsg("§6[Server]", "A boss has spawned in the Nether!")

# Send individual messages to every online player
# Player IDs are stored in the system; track them on AddServerPlayerEvent/DelServerPlayerEvent
self.onlinePlayers = set()
def OnPlayerJoin(self, args):
    self.onlinePlayers.add(args["id"])
def OnPlayerLeave(self, args):
    self.onlinePlayers.discard(args["id"])
def broadcast(self, msg, color="§e"):
    for pid in self.onlinePlayers:
        serverApi.GetEngineCompFactory().CreateMsg(pid).NotifyOneMessage(pid, msg, color)
```

> Player IDs look like negative longs: `"-4294967295"`. Always store and compare as `str`.

## Critical Conventions

### Python 2 Required

The engine runs **Python 2**, NOT Python 3. All code must be Python 2 compatible:
- Use `# -*- coding: utf-8 -*-` header for Chinese strings
- Use `ServerSystem.__init__(self, namespace, systemName)` not `super()`
- Use `dict.get("key", default)` for safe access
- Install: `python -m pip install mc-netease-sdk` (for IDE autocomplete)

### Server/Client Isolation (CRITICAL)

- **Never** import `server.*` in client scripts or `client.*` in server scripts
- Server events/components don't work on client side, and vice versa
- Cross-side communication uses **custom events** only: `NotifyToClient`, `NotifyToServer`

### RegisterSystem Takes a STRING PATH

```python
# CORRECT: String path from scripts root
serverApi.RegisterSystem("MyMod", "MyServerSystem",
    "MyModScripts.myServerSystem.MyServerSystem")

# WRONG: Class instance
serverApi.RegisterSystem("MyMod", "MyServerSystem", MyServerSystem("MyMod", "MyServerSystem"))
```

### modMain.py Must Be at Root of Scripts Folder

> "因为会基于该文件的层级进行打包，所以该文件必须放在Scripts文件夹下，否则会导致Mod在手机端失效"

### Logging

Use `mod_log` module, NOT bare `print`:
```python
from mod_log import logger
logger.info("[ModPrefix] message: %s", variable)
```
Logs appear in MC Studio's "脚本测试日志" (Script Test Log) window.

### Debugging

No IDE breakpoint support. Debug entirely via log output (`from mod_log import logger`). Hot-reload works for **function body changes** only — global variables, new classes, or new files require re-entering the world.

**Diagnostic workflow:**
1. First, verify `__init__` runs: log at entry
2. Verify `ListenEvent` runs: log at entry/exit. **If missing, add `self.ListenEvent()` call in `__init__`**
3. Verify events fire: log in each callback with the received args
4. Verify API calls work: log return values
5. For tick-dependent logic, add a first-tick check: `if self.globalTick == 1: ...`

**Testing the event system (minimal diagnostic):**
```python
def OnTick(self, args=None):
    logger.info("[Mod] TICK")  # Should flood logs if working

def OnPlayerJoin(self, args):
    logger.info("[Mod] JOIN: %s", args)  # May not fire in singleplayer
```

### DestroyServer / DestroyClient

These lifecycle hooks are for cleanup — use them when you need to:
- Save persistent data before the world unloads: `CreateExtraData().SaveExtraData()`
- Restore game rules to defaults: `SetCommand("/gamerule doDaylightCycle true")`
- Clean up custom entities or structures you created in `InitServer`

```python
@Mod.DestroyServer()
def serverDestroy(self):
    comp = serverApi.GetEngineCompFactory().CreateExtraData(serverApi.GetLevelId())
    comp.SaveExtraData()
    logger.info("[MyMod] Data saved on shutdown")
```

### Server → All Clients Broadcast

```python
# Server side: send to all online players at once
self.BroadcastToAllClient("MyMod_Announce", {"title": "Event!", "body": "A wild boss appears!"})

# Client side: each client listens normally
def ListenEvent(self):
    self.ListenForEvent("MyMod", "MyClientSystem", "MyMod_Announce", self, self.OnAnnounce)
```

Unlike `NotifyToClient(playerId, ...)` which targets one player, `BroadcastToAllClient` sends to every connected client.

## Common Mistakes & Troubleshooting

### Coding Mistakes

| Mistake | Symptom | Fix |
|---|---|---|
| Passing class instance to `RegisterSystem` | Mod silently not loading | Pass string path: `"ScriptsFolder.file.ClassName"` |
| Importing `serverApi` in client scripts | Component not working, event not firing | Use `clientApi` for client-side imports |
| Python 3 syntax (f-strings, `super()`) | `SyntaxError` in game log | Use `%s` formatting, explicit `__init__` call |
| Forgetting `__init__.py` | `ImportError: No module named XxxScripts` | Create empty `__init__.py` in scripts folder |
| Naming scripts folder too generically | Name collision with other mods | Use `[TeamName][ModName]Scripts` pattern |
| Cross-importing ServerSystem ↔ ClientSystem | Silent failure on one side | Use custom events: `NotifyToClient` / `NotifyToServer` |
| Nesting `modMain.py` in a subfolder | Mod works on PC, fails on mobile | Place `modMain.py` directly in `[Name]Scripts/` |
| Using bare `print` for logging | Can't find output; log window empty | Use `from mod_log import logger` |

### Runtime Errors

| Symptom | Likely Cause | Fix |
|---|---|---|
| `ImportError: No module named mod_log` | Python env missing SDK stub | `pip install mc-netease-sdk` for Python 2 |
| `System not registered` or mod not loading | `RegisterSystem` path mismatch | Verify: `ScriptsFolder.fileName.ClassName` triple match |
| Hot-reload doesn't pick up changes | Changed global var, new class, or new file | Save → exit to menu → re-enter world |
| `AttributeError: 'NoneType' has no attribute` | `GetComponent` returned None | Use `CreateComponent` (auto-gets if exists) |
| Event callback never fires | `ListenEvent` not called by engine (common) | **Call `self.ListenEvent()` manually in `__init__`** |
| `OnTick() takes exactly 2 arguments (1 given)` | Tick callback signature wrong | Use `def OnTick(self, args=None)` — engine passes 0 args |
| `KeyError` in event callback | Event param name differs | Check [[api-events]]; `id` vs `playerId` varies per event |
| `No module named <version>.weightServerSystem` | `__file__` returns engine version string | Hardcode scripts folder name, don't use `__file__` |
| `SyntaxError: 'utf8' codec can't decode byte 0xa7` | `\xa7` in source is invalid UTF-8 byte | Use `unichr(167)` or skip color codes in source |
| `[WARNING] get pos type wrong!` when reading inventory | Using `GetEntityItem` for player inventory | Use `GetPlayerAllItems`/`GetPlayerItem` instead |
| `AddServerPlayerEvent` not firing | Player created before scripts load (singleplayer) | Also listen for `AddEntityServerEvent` with `engineTypeStr` check |
| AddEffectToEntity has no effect | Parameter order wrong: `(name, amp, dur)` | Correct order: `(effectName, duration, amplifier, showParticles)` |
| Chinese text garbled in logs | Missing UTF-8 header | Add `# -*- coding: utf-8 -*-` at top of every file |
| Works in editor, broken after publish | UUID collision or path >150 chars | Regenerate UUIDs; shorten directory nesting |
| Upload "打包失败" | Python syntax error or path depth | Fix syntax; rename deep folders |
| Mobile load failure | Chinese/funky filenames in packs | ASCII-only filenames in behavior_pack/resource_pack |
| `JSON format error (code 3001/3002)` | Malformed JSON in config | VSCode shows red squiggles at error position |
| `Custom item ID error (code 5001)` | Identifier missing namespace | Must be `"mymod:item_name"` not `"item_name"` |
| `ItemDict` silently not working | Wrong key names | Use `newItemName`/`newAuxValue` (modern) or `itemName`/`auxValue` (legacy) |
| `GetEntitiesInSphereArea` not found | Method name may differ by SDK version | Test with try/except; use `AddEntityServerEvent` as fallback |

## Color Codes

| Code | Effect | Code | Effect |
|---|---|---|---|
| `§0`-`§f` | 16 colors (0=black, f=white) | `§l` | **Bold** |
| `§a` | Green | `§c` | Red |
| `§e` | Yellow | `§6` | Gold |
| `§k` | Obfuscated | `§m` | ~~Strikethrough~~ |
| `§n` | <u>Underline</u> | `§o` | *Italic* |
| `§r` | Reset all formatting | | |

> `"§c§lBOSS DEFEATED!§r"` = bold red, then reset. Full table: [[api-components]] Enums.

## Manifest Format

```json
{
    "format_version": 1,
    "header": {
        "description": "Description text",
        "name": "ModName",
        "uuid": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
        "version": [1, 0, 0],
        "min_engine_version": [1, 18, 0]
    },
    "modules": [{
        "type": "script",
        "language": "python",
        "uuid": "YYYYYYYY-YYYY-YYYY-YYYY-YYYYYYYYYYYY",
        "version": [1, 0, 0],
        "entry": "ModNameScripts/modMain.py"
    }],
    "dependencies": []
}
```

Header UUID and module UUID must be **different**. Generate them via MC Studio or `uuidgen`.
