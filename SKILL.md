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
https://raw.githubusercontent.com/MIKE63454/-netease-mc-mod-dev-skill/main/我的世界开发者文档/[path-to-file].md
```
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
            {"itemName": "minecraft:diamond_sword", "count": 1, "auxValue": 0},
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

def OnTick(self, args):
    # Called 30 times per second
    # Keep logic lightweight — heavy ops cause lag
    pass
```

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

## Commonly Used Events

### Server-Side Events (Top 8)

| Event Name | Trigger | Key args |
|---|---|---|
| `AddServerPlayerEvent` | Player joins | `id` (playerId) |
| `ServerChatEvent` | Player sends chat | `message`, `playerId`, `cancel`, `bChatById` |
| `PlayerHurtEvent` | Player takes damage | `id`, `attacker`, `cause` |
| `PlayerDieEvent` | Player dies | `id`, `attacker`, `cause` |
| `PlayerRespawnFinishServerEvent` | Player respawn complete (safe) | `playerId` |
| `ServerBlockUseEvent` | Player interacts with block | `blockName`, `playerId`, `x/y/z` |
| `AddEntityServerEvent` | Entity created/loaded | `id`, `engineTypeStr`, `posX/Y/Z` |
| `ClientLoadAddonsFinishServerEvent` | Client mod loading done | — trigger client data init |

### Client-Side Events (Top 3)

| Event Name | Trigger | Key args |
|---|---|---|
| `OnLocalPlayerStopLoading` | Player fully loaded | `id` |
| `AddEntityClientEvent` | Entity created (client) | `id`, `engineTypeStr` |
| `OnScriptTickClient` | Every frame (variable) | — |

> Full event reference with all parameters: see [[api-events]]

## Color Codes

Use Minecraft format codes (string with `§` prefix):

| Code | Effect | Code | Effect |
|---|---|---|---|
| `§0` | Black | `§1` | Dark Blue |
| `§2` | Dark Green | `§3` | Dark Aqua |
| `§4` | Dark Red | `§5` | Dark Purple |
| `§6` | Gold | `§7` | Gray |
| `§8` | Dark Gray | `§9` | Blue |
| `§a` | Green | `§b` | Aqua |
| `§c` | Red | `§d` | Light Purple |
| `§e` | Yellow | `§f` | White |
| `§l` | **Bold** | `§m` | ~~Strikethrough~~ |
| `§n` | <u>Underline</u> | `§o` | *Italic* |
| `§k` | Obfuscated | `§r` | Reset |

> Example: `"§c§lBOSS DEFEATED!§r"` → bold red text, then reset.

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

No IDE breakpoint support. Debug entirely via log output. Hot-reload works for **function body changes** only — global variables, new classes, or new files require re-entering the world.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Passing class instance to `RegisterSystem` | Pass a string module path |
| Importing `serverApi` in client scripts | Use `clientApi` for client-side imports |
| Using Python 3 syntax (f-strings, `super()`) | Use `%s` formatting, explicit `__init__` call |
| Forgetting `__init__.py` | Always create empty `__init__.py` |
| Naming scripts folder too generically | Use `[TeamName][ModName]Scripts` pattern |
| Cross-importing ServerSystem from ClientSystem | Use custom events for cross-side communication |
| Nesting `modMain.py` in a subfolder | Place it directly in the scripts root folder |
| Using bare `print` for logging | Use `from mod_log import logger` |

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| `ImportError: No module named mod_log` | Python env missing SDK stub | `pip install mc-netease-sdk` in Python 2 |
| `System not registered` or mod not loading | `RegisterSystem` path doesn't match file structure | Verify triple match: `ScriptsFolder.fileName.ClassName` |
| Mod works on PC but not mobile | `modMain.py` not at scripts root | Move `modMain.py` directly into `[Name]Scripts/` folder |
| Hot-reload doesn't pick up changes | Changed global var, new class, or new file | Save → exit to menu → re-enter world |
| `AttributeError: 'NoneType' object has no attribute` | `GetComponent` returned None | Check entity exists; use `CreateComponent` first |
| Event callback never fires | Wrong event name or system name | Use exact event name from docs; verify `GetEngineSystemName()` |
| Server component not working on client | Server/client isolation violated | Move logic to correct side; use custom events if cross-side |
| `KeyError` in event callback | Event param name differs from docs | Check [[api-events]] for exact parameter names |
| Chinese text garbled in logs | Missing UTF-8 header | Add `# -*- coding: utf-8 -*-` at top of file |
| Mod works in editor but not after publish | UUID collision or missing files | Regenerate UUIDs; verify all files included in pack |

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
