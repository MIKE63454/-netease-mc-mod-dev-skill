# NetEase Mod SDK — Event Reference

## Server-Side Events

### World Events (世界)

| Event | Trigger | Key Parameters |
|---|---|---|
| `AddServerPlayerEvent` | Player joins game | `id`(str): playerId |
| `DelServerPlayerEvent` | Player removed | `id`(str): playerId |
| `ServerChatEvent` | Player sends chat message | `username`(str), `playerId`(str), `message`(str), `cancel`(bool), `bChatById`(bool) |
| `ClientLoadAddonsFinishServerEvent` | Client finishes loading mods | (no key params, use to send init data to client) |
| `ChunkGeneratedServerEvent` | Chunk generation complete | `dimensionId`(int), `chunkPosX`(int), `chunkPosZ`(int) |
| `ChunkLoadedServerEvent` | Chunk loaded | `dimensionId`(int), `chunkPosX`(int), `chunkPosZ`(int) |
| `EntityRemoveEvent` | Entity removed | `id`(str): entityId |
| `AddEntityServerEvent` | Entity created/loaded from save | `id`(str), `posX`(float), `posY`(float), `posZ`(float), `dimensionId`(int), `isBaby`(bool), `engineTypeStr`(str) |
| `ServerSpawnMobEvent` | Mob spawned (auto or API) | `entityId`(str), `engineTypeStr`(str) |
| `ExplosionServerEvent` | Explosion occurs | `center`(tuple), `radius`(float) |
| `CommandEvent` | Player executes command | `playerId`(str), `command`(str) |
| `PlayerJoinMessageEvent` | "X joined game" message about to display | `playerId`(str) |
| `PlayerLeftMessageServerEvent` | "X left game" message about to display | `playerId`(str) |
| `OnScriptTickServer` | Server tick (30/sec) | (none) |
| `LoadServerAddonScriptsAfter` | Server done loading mods | (none) |

### Player Events (玩家)

| Event | Trigger | Key Parameters |
|---|---|---|
| `PlayerDieEvent` | Player dies | `id`(str), `attacker`(str), `cause`(str), `customTag`(str) |
| `PlayerRespawnEvent` | Player respawns (respawn button) | `id`(str) — **Note:** may fire before player is fully respawned, prefer `PlayerRespawnFinishServerEvent` |
| `PlayerRespawnFinishServerEvent` | Player respawn complete (safe to teleport/set health) | `playerId`(str) |
| `PlayerHurtEvent` | Player takes damage (before damage applies) | `id`(str), `attacker`(str), `cause`(str), `customTag`(str) |
| `PlayerEatFoodServerEvent` | Player eats food | `playerId`(str), `itemDict`(dict), `hunger`(int, modifiable), `nutrition`(float, modifiable) |
| `PlayerAttackEntityEvent` | Player attacks entity | `playerId`(str), `victimId`(str), `itemDict`(dict) |
| `PlayerTeleportEvent` | Player teleports (ender pearl, /tp) | `playerId`(str), `posX/Y/Z`(float) |
| `AddExpEvent` | Player gains experience | `id`(str), `addExp`(int) |
| `AddLevelEvent` | Player levels up | `id`(str) |
| `DimensionChangeServerEvent` | Player dimension changing | `playerId`(str), `dimensionId`(int) |
| `PlayerInteractServerEvent` | Player can interact with entity | `playerId`(str), `itemDict`(dict), `victimId`(str), `cancel`(bool) |
| `PlayerDoInteractServerEvent` | Player interacts with entity | `playerId`(str), `itemDict`(dict), `interactEntityId`(str) |
| `PlayerSleepServerEvent` | Player sleeps successfully | `playerId`(str) |
| `GameTypeChangedServerEvent` | Game mode changes | `playerId`(str), `gameType`(int) |

### Block/Item Events (方块/物品)

| Event | Trigger | Key Parameters |
|---|---|---|
| `ServerBlockUseEvent` | Player right-clicks / interacts with block | `blockName`(str), `playerId`(str), `x/y/z`(int) |
| `ServerBlockEntityTickEvent` | Block entity tick | `blockName`(str), `pos`(dict) |

### Entity Events (实体)

| Event | Trigger | Key Parameters |
|---|---|---|
| `OnFireHurtEvent` | Entity takes fire damage | `id`(str), `ignite`(bool) |
| `AddEntityServerEvent` | Entity created/loaded | `id`(str), `engineTypeStr`(str), `posX/Y/Z`(float) |

---

## Client-Side Events

| Event | Trigger | Key Parameters |
|---|---|---|
| `OnLocalPlayerStopLoading` | Local player fully loaded (safe to switch dimension) | `id`(str) |
| `AddEntityClientEvent` | Entity created on client | `id`(str), `engineTypeStr`(str), `posX/Y/Z`(float) |
| `RemoveEntityClientEvent` | Entity removed on client | `id`(str) |
| `AddPlayerAOIClientEvent` | Player enters local player's view | `playerId`(str) |
| `RemovePlayerAOIClientEvent` | Player leaves local player's view | `playerId`(str) |
| `OnScriptTickClient` | Client render frame (variable rate) | (none) |
| `OnCommandOutputClientEvent` | Command has success output | (command output params) |
| `LoadClientAddonScriptsAfter` | Client mod load complete | (none) |
| `UiInitFinished` | UI init done | (none) |

---

## Event Patterns

### Listening to Server Events

```python
import server.extraServerApi as serverApi

def ListenEvent(self):
    self.ListenForEvent(
        serverApi.GetEngineNamespace(), serverApi.GetEngineSystemName(),
        "EventName",
        self, self.CallbackMethod
    )

def CallbackMethod(self, args):
    # args is a dict with event-specific parameters
    playerId = args.get("id", "")
    # Some args are modifiable to change game behavior
    # e.g., args["cancel"] = True to prevent an action
```

### Listening to Client Events

```python
import client.extraClientApi as clientApi

def ListenEvent(self):
    self.ListenForEvent(
        clientApi.GetEngineNamespace(), clientApi.GetEngineSystemName(),
        "EventName",
        self, self.CallbackMethod
    )
```

### Cross-Side Communication

Server → Client:
```python
# Server side
self.NotifyToClient(playerId, "CustomEventName", {"key": "value"})

# Client side: Listen for the same custom event
```

Client → Server:
```python
# Client side
self.NotifyToServer("CustomEventName", {"key": "value"})

# Server side: Listen for the same custom event
```
