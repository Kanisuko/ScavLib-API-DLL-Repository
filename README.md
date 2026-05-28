## **`README.md`**

```markdown
# ScavLib — Scav Prototype Mod API Library

**Version:** 0.4.2

**Required by:** Any mod built on ScavLib  

**Nexus Mods:** [Nexus Mods Page](https://www.nexusmods.com/scavprototype/mods/83)  

---

## Description

ScavLib is a foundational developer-focused API and utility library designed to
simplify mod development for **Scav Prototype**. It streamlines common tasks such
as building in-game menus, managing configurations, registering console commands,
handling custom events, interacting safely with the game state, and working with
player state, skills, limbs, and items.

> **Note for players:** This is a core dependency library. It does not add any
> content on its own, but is required by other mods to function.

---

## What's New in 0.4.2

- **Hierarchical subcommands** — `BaseCommand.SubCommands` + `ExecuteSubCommand()`
  for structured command routing with automatic Tab completion and help output.
- **Command owner ledger** — `CommandRegistry.TryRegister()` tracks which mod
  owns each command; `Unregister()` safely removes only ScavLib-registered commands.
- **`scavlib check`** — new diagnostic subcommand reporting patch status and
  command ownership.
- **Patch failure isolation** — a single failing Harmony patch no longer takes
  down the entire plugin.
- **`GameUtil.Log` multi-line support** — embedded newlines now render as
  separate console lines.

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
   to the console. Mods registered with a lifecycle object are annotated with `[F]`.

   For a full diagnostic (patch status, command owners), run:
   ```
   scavlib check
   ```

---

## Requirements

- **BepInEx 5.x** (64-bit version recommended)
- Scav Prototype game client (on Steam)

---

## For Developers

Full API documentation is available on the [Wiki](https://github.com/Kanisuko/ScavLib-API-DLL-Repository/wiki).
All public methods carry XML doc comments visible in any IDE with IntelliSense support.

To reference ScavLib in your own mod project, add `ScavLib.dll` as an assembly
reference and declare the dependency in your BepInEx plugin attribute:

```csharp
[BepInDependency("com.kanisuko.scavlib", "0.4.2")]
[BepInPlugin("com.yourname.yourmod", "YourMod", "1.0.0")]
public class YourPlugin : BaseUnityPlugin { ... }
```