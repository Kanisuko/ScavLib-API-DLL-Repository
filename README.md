# ScavLib — Scav Prototype Mod API Library

**Version:** 0.3.0

**Required by:** Any mod built on ScavLib  

**Nexus Mods:** [Nexus Mods Page](https://www.nexusmods.com/scavprototype/mods/83)  

---

## Description

ScavLib is a foundational developer-focused API and utility library designed to
simplify mod development for **Scav Prototype**. It streamlines common tasks such
as building in-game menus, managing configurations, registering console commands,
handling custom events, interacting safely with the game state, and working with
player state, skills, limbs, and items.

Version 0.3.0 expands `PlayerUtil` to full coverage of the player `Body` — over
70 read/write methods spanning vitals, circulation, neurology, toxicology,
immunity, and external conditions — alongside a `Thresholds` constants class that
mirrors every moodle trigger value used by the game's own UI.

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

## For Developers

Full API documentation and usage examples will accompany the source code
release. In the meantime, all public methods carry XML doc comments visible
in any IDE with IntelliSense support.

To reference ScavLib in your own mod project, add `ScavLib.dll` as an assembly
reference and declare the dependency in your BepInEx plugin attribute:

```csharp
[BepInDependency("com.kanisuko.scavlib", "0.3.0")]
[BepInPlugin("com.yourname.yourmod", "YourMod", "1.0.0")]
public class YourPlugin : BaseUnityPlugin { ... }
```