# NetEase Mod SDK — Event Reference

## Server-Side Events

### World Events (世界)

| Event | Trigger | Key Parameters |
|---|---|---|
| `AddServerPlayerEvent` | Player joins game | `id`(str): playerId. **May NOT fire in singleplayer — player entity created before scripts load.** Use `AddEntityServerEvent` as fallback. |
| `DelServerPlayerEvent` | Player removed | `id`(str): playerId |
| `ServerChatEvent` | Player sends chat message | `username`(str), `playerId`(str), `message`(str), `cancel`(bool), `bChatById`(bool) |
| `ClientLoadAddonsFinishServerEvent` | Client finishes loading mods | (no key params, use to send init data to client) |
| `ChunkGeneratedServerEvent` | Chunk generation complete | `dimensionId`(int), `chunkPosX`(int), `chunkPosZ`(int) |
| `ChunkLoadedServerEvent` | Chunk loaded | `dimensionId`(int), `chunkPosX`(int), `chunkPosZ`(int) |
| `EntityRemoveEvent` | Entity removed | `id`(str): entityId |
| `AddEntityServerEvent` | Entity created/loaded from save | `id`(str), `posX`(float), `posY`(float), `posZ`(float), `dimensionId`(int), `isBaby`(bool), `engineTypeStr`(str). **`engineTypeStr` = "minecraft:player" for players — reliable singleplayer fallback.** |
| `ServerSpawnMobEvent` | Mob spawned (auto or API) | `entityId`(str), `engineTypeStr`(str) |
| `ExplosionServerEvent` | Explosion occurs | `center`(tuple), `radius`(float) |
| `CommandEvent` | Player executes command | `playerId`(str), `command`(str) |
| `PlayerJoinMessageEvent` | "X joined game" message about to display | `playerId`(str) |
| `PlayerLeftMessageServerEvent` | "X left game" message about to display | `playerId`(str) |
| `OnScriptTickServer` | Server tick (30/sec) | **(none — callback receives 0 args). CRITICAL: use `def OnTick(self, args=None)`** |
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

**Item Events:**

| Event | Trigger | Key Parameters |
|---|---|---|
| `ServerItemUseOnEvent` | Player right-clicks on block with item | `entityId`, `itemDict`, `x/y/z`, `blockName`, `ret`(bool, modifiable) |
| `ActorUseItemServerEvent` | Player uses item | `playerId`, `itemDict`, `useMethod` |
| `ServerItemTryUseEvent` | Player attempts to use item (cancelable) | `playerId`, `itemDict`, `cancel`(bool) |
| `ActorAcquiredItemServerEvent` | Player acquires any item | `playerId`, `itemDict` |
| `OnCarriedNewItemChangedServerEvent` | Main-hand item swap | `playerId`, `oldItem`, `newItem` |
| `ItemDurabilityChangedServerEvent` | Item durability changes (modifiable) | `entityId`, `itemDict`, `durability`(int), `canChange`(bool) |

**Block Events:**

| Event | Trigger | Key Parameters |
|---|---|---|
| `ServerBlockUseEvent` | Player right-clicks / interacts with block | `blockName`(str), `playerId`(str), `x/y/z`(int) |
| `ServerPlayerTryDestroyBlockEvent` | Player about to destroy block (cancelable) | `x/y/z`, `fullName`, `playerId`, `cancel`(bool), `spawnResources`(bool) |
| `ServerEntityTryPlaceBlockEvent` | Entity about to place block (cancelable) | `x/y/z`, `fullName`, `entityId`, `cancel`(bool) |
| `EntityPlaceBlockAfterServerEvent` | After block placed | `x/y/z`, `fullName`, `entityId` |
| `BlockRandomTickServerEvent` | Block random tick (custom crops) | `x/y/z`, `fullName`, `blockName` |
| `ServerBlockEntityTickEvent` | Block entity tick | `blockName`(str), `pos`(dict) |

### Entity Events (实体)

| Event | Trigger | Key Parameters |
|---|---|---|
| `OnFireHurtEvent` | Entity takes fire damage | `id`(str), `ignite`(bool) |

> Note: `AddEntityServerEvent` (see World Events) also fires for entities. The engine does not have a generic `EntityHurtEvent`; use `PlayerHurtEvent` for player damage and `OnFireHurtEvent` for fire damage to mobs.

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

### Key Parameter Naming Inconsistency (IMPORTANT)

The engine uses different parameter names for the same concept across events. Always check the exact parameter name for each event:

| Event | Player ID param | Entity ID param |
|---|---|---|
| `AddServerPlayerEvent` | `id` | — |
| `ServerChatEvent` | `playerId` | — |
| `PlayerDieEvent` | `id` | `attacker` |
| `PlayerHurtEvent` | `id` | `attacker` |
| `PlayerRespawnEvent` | `id` | — |
| `PlayerRespawnFinishServerEvent` | `playerId` | — |
| `ServerBlockUseEvent` | `playerId` | — |

> **Rule:** Always use `args.get("id")` for `*Player*` events but check `api-events.md` for the exact name. Using the wrong key silently returns `None` instead of throwing an error.

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

See SKILL.md "Server ↔ Client Communication" section for complete examples with both sides' listening code.
