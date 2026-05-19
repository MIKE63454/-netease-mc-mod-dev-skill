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

## Commonly Used Events

### Server-Side Events

| Event Name | Trigger | Key args |
|---|---|---|
| `AddServerPlayerEvent` | Player joins | `id` (playerId) |
| `ServerChatEvent` | Player sends chat | `message`, `playerId` |
| `PlayerRespawnEvent` | Player respawns | `id`, `name` |
| `PlayerDieEvent` | Player dies | `id` |
| `ServerBlockUseEvent` | Player interacts with block | `blockName`, `playerId` |
| `EntityHurtEvent` | Entity takes damage | `id`, `srcId`, `damage` |
| `AddEntityServerEvent` | Entity created/loaded | `id`, `engineTypeStr` |
| `ClientLoadAddonsFinishServerEvent` | Client mod loading done | — trigger client data init |

### Client-Side Events

| Event Name | Trigger | Key args |
|---|---|---|
| `OnLocalPlayerStopLoading` | Player fully loaded | `id` |
| `AddEntityClientEvent` | Entity created (client) | `id`, `engineTypeStr` |
| `OnScriptTickClient` | Every frame (30/sec) | — |

## Color Codes

Use Minecraft format codes (string with `§` prefix):

| Code | Color | Code | Color |
|---|---|---|---|
| `§a` | Green | `§c` | Red |
| `§e` | Yellow | `§b` | Aqua |
| `§7` | Gray | `§f` | White |
| `§l` | **Bold** | `§r` | Reset |

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
