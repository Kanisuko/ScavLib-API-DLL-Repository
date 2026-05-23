# ScavLib — Scav Prototype Mod API Library

**Version:** 0.2.0  
**Required by:** Any mod built on ScavLib  
**Nexus Mods:** [Nexus Mods Page](https://www.nexusmods.com/scavprototype/mods/83)  
> Full source code will be released once the API reaches a stable milestone.

---

## Description

ScavLib is a foundational developer-focused API and utility library designed to
simplify mod development for **Scav Prototype**. It streamlines common tasks such
as building in-game menus, managing configurations, registering console commands,
handling custom events, interacting safely with the game state, and working with
player vitals, skills, and items.

> **Note for players:** This is a core dependency library. It does not add any
> content on its own, but is required by other mods to function.

---

## Installation

1. Ensure you have **BepInEx 5** installed for Scav Prototype.
2. Download `ScavLib.dll`.
3. Place `ScavLib.dll` into your game directory under `BepInEx/plugins/`.
4. Launch the game. To verify it loaded correctly, open the developer console
   and type:
   ```
   scavlib status
   ```
   You should see ScavLib's version, authors, and all registered mods printed
   to the console.

---

## Requirements

- **BepInEx 5.x** (64-bit version recommended)
- Scav Prototype game client (on Steam)

---

## Features

### IMGUI Menu Framework — `MenuWindow` / `MenuBuilder`

Inherit `MenuWindow` to create custom in-game movable windows with a single
method override. Use `MenuBuilder` to draw buttons, sliders, toggles, labels,
and separators, with built-in helpers for auto-binding BepInEx `ConfigEntry`
values directly to UI controls.

Windows with `Height = 0` (the default) now use **true auto-height** — the
window automatically resizes to fit its content every frame.

---

### Simplified Config Manager — `ConfigManager`

A lightweight wrapper around BepInEx's `ConfigFile` that reduces boilerplate
when declaring and binding config entries. Supports optional `AcceptableValueBase`
ranges and full `ConfigDescription` control.

---

### Decoupled Event Bus — `EventBus`

Implement event-driven patterns with lightweight, attribute-based event
subscriptions using `[Subscribe]`. Dispatch inherits up the type hierarchy —
subscribing to a base event type will catch all derived events.

**0.2.0 pre-defined game events** (auto-triggered via Harmony, no manual
patching required):

| Event | Trigger |
|---|---|
| `WorldLoadedEvent` | World fully loaded, `Body` is safe to access |
| `ItemPickedUpEvent` | Item successfully placed in an inventory slot |
| `ItemDroppedEvent` | Item removed from inventory and dropped in world |

Example:
```csharp
// In your plugin's Awake():
EventBus.Register(this);

[Subscribe]
private void OnWorldLoaded(WorldLoadedEvent e)
{
    GameUtil.Alert("World ready!");
}

[Subscribe]
private void OnItemPickedUp(ItemPickedUpEvent e)
{
    GameUtil.Log($"Picked up: {e.ItemId} in slot {e.Slot}");
}
```

---

### Console Command Registry — `CommandRegistry`

Safely register custom console commands into the game's native developer
console, with support for argument auto-fill suggestions and per-argument
descriptions. Commands are injected at the correct time automatically — no
manual timing management required.

---

### Player Utilities — `PlayerUtil`

Safe, null-protected wrappers around the player's `Body` component.

**Vital sign reads** (all return `0` / `false` when no world is loaded):
`GetBloodOxygen()`, `GetBloodVolume()`, `GetHeartRate()`, `GetBloodPressure()`,
`GetTemperature()`, `GetHunger()`, `GetThirst()`, `GetStamina()`, `GetEnergy()`,
`GetConsciousness()`, `GetBrainHealth()`, `GetHappiness()`

**State queries:**
`IsAlive()`, `IsConscious()`, `IsDying()`, `IsCriticallyDying()`,
`IsInCardiacArrest()`, `IsSleeping()`, `IsExercising()`

**Recommended writes** (respect game logic — animations, sounds, side effects):
`Feed()`, `Hydrate()`, `RestoreStamina()`, `RestoreEnergy()`, `HealAll()`

**Raw writes** (direct field writes with Clamp, bypass game logic — use with
caution, suffix `Raw` signals intent):
`SetHungerRaw()`, `SetThirstRaw()`, `SetStaminaRaw()`, `SetEnergyRaw()`,
`SetBloodVolumeRaw()`, `SetBloodOxygenRaw()`, `SetTemperatureRaw()`,
`SetBrainHealthRaw()`, `SetConsciousnessRaw()`

**Inventory shortcuts:**
`FindItemById()`, `FindItemByTag()`, `GiveItem()`

**`PlayerUtil.Thresholds`** — a nested static class containing ~50 named
constants extracted from the game's internal moodle system (blood pressure,
blood oxygen, heart rate, temperature, bleeding speed, hunger, thirst, stamina,
energy, consciousness, pain, brain health, etc.). Use these instead of
hard-coding magic numbers to stay consistent with the game's own UI.

---

### Skill Utilities — `SkillUtil`

Safe wrappers around the player's `Skills` component, using a typed
`SkillType` enum (`Strength`, `Resilience`, `Intelligence`) instead of the
game's internal magic integers.

- `GetLevel(SkillType)` — current skill level
- `GetProgress(SkillType)` — normalized 0~1 progress toward next level
- `GetExperience(SkillType)` — raw XP value
- `GetExperienceInLevel(SkillType)` / `GetExperienceForNextLevel(SkillType)`
- `AddExperience(SkillType, float)` — triggers level-up alerts and sounds
- `SetLevelRaw(SkillType, int)` — direct level set, no alerts
- `XpMultiplier` — get/set the global XP gain multiplier (`Skills.xpGainMult`)

---

### Item Utilities — `ItemUtil`

World-space item helpers, complementing `PlayerUtil`'s inventory-focused API.

- `FindNearby(Vector2, float)` — find all items within a radius
- `FindClosest(Vector2, float)` — find the nearest item
- `SetCondition(Item, float)` — safe condition set (handles liquid containers)
- `Repair(Item)` — restore item to full condition
- `SetFavourited(Item, bool)` — toggle favourite flag
- `Destroy(Item)` — safely remove an item from the world
- `GetInfo(string id)` — look up an `ItemInfo` from the global registry
- `IsKnownId(string id)` — check if an item ID is registered
- `GetAllIds()` — enumerate all registered item IDs

---

### Custom Item Registry — `CustomItemRegistry`

Register custom `ItemInfo` definitions into the game's global item registry
(`Item.GlobalItems`). Injection happens automatically after the game's own
`SetupItems()` runs — registration calls made before world load are safely
queued.

```csharp
// Simple registration:
CustomItemRegistry.RegisterSimpleItem("mymod_herb", category: "custom",
    weight: 0.3f, value: 5, tags: "medicine");

// Full control:
var info = new ItemInfo { ... };
CustomItemRegistry.RegisterItem("mymod_herb", info);
```

> **Current limitation (0.2.0):** Registering an `ItemInfo` adds the item's
> *definition* to the registry, but does not create a spawnable Prefab.
> `Utils.Create("mymod_herb", ...)` will not yet work. Spawnable custom items
> are planned for a future version once an AssetBundle loading layer is added.

---

### Safe Game Utilities — `GameUtil`

Fail-safe helpers for common game-state operations. All methods are safe to
call from the main menu.

- `IsInGame` / `IsWorldLoaded` — world state checks
- `GetBody()` / `TryGetBody()` — player Body access
- `GetPlayerPosition()` — world-space player position
- `SpawnItem(id, position)` / `SpawnItemAt(id, Transform)` — world spawning
- `SpawnAtPlayer(id)` — spawn and auto-pickup at player feet
- `Log(text)` — write to in-game developer console
- `Alert(text, important)` — show in-game alert popup
- `Notify(text)` — alert + console log in one call
- `IsPointerOverUI()` — UI raycast check for menu input handling

---

### Mod Registry — `ModRegistry`

Register your mod's metadata so other mods and ScavLib itself can discover it.

```csharp
ModRegistry.Register(new ModInfo("MyMod", "1.0.0", "Does cool things.", "YourName"));
```

- `GetAll()` — list all registered mods
- `TryFind(name, out ModInfo)` — look up a mod by name
- `IsRegistered(name)` — quick existence check

---

## For Developers

Full API documentation and usage examples will accompany the source code
release. In the meantime, all public methods carry XML doc comments visible
in any IDE with IntelliSense support.

To reference ScavLib in your own mod project, add `ScavLib.dll` as an assembly
reference and declare the dependency in your BepInEx plugin attribute:

```csharp
[BepInDependency("com.kanisuko.scavlib", "0.2.0")]
[BepInPlugin("com.yourname.yourmod", "YourMod", "1.0.0")]
public class YourPlugin : BaseUnityPlugin { ... }
```

---

## Changelog

### 0.2.0
- Added `PlayerUtil` with vital sign reads, state queries, recommended writes,
  raw writes, inventory shortcuts, and `PlayerUtil.Thresholds` constants
- Added `SkillUtil` with typed `SkillType` enum and full skill read/write API
- Added `ItemUtil` for world-space item scanning, condition management, and
  global registry queries
- Added `CustomItemRegistry` for registering custom `ItemInfo` definitions
- Added pre-defined game events: `WorldLoadedEvent`, `ItemPickedUpEvent`,
  `ItemDroppedEvent` with automatic Harmony-based triggering
- Upgraded `EventBus.Post<T>` to dispatch along the full inheritance chain
- Fixed `MenuWindow` auto-height (was hardcoded 400px fallback, now true IMGUI
  auto-height)
- Fixed `MenuManager` erroneous `using UnityEngine.Windows` reference
- Added `ModRegistry.TryFind()` and `IsRegistered()`
- Added `GameUtil.IsWorldLoaded`, `SpawnItemAt()`, `Notify()`
- Cleaned up `CommandRegistry.Init()` dead code

### 0.1.0
- Initial release: MenuWindow/MenuBuilder, ConfigManager, EventBus,
  CommandRegistry, GameUtil, ModRegistry