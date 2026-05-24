# ScavLib — Scav Prototype Mod API Library

**Version:** 0.2.3
**Required by:** Any mod built on ScavLib  
**Nexus Mods:** [Nexus Mods Page](https://www.nexusmods.com/scavprototype/mods/83)  
> Full source code will be released once the API reaches a stable milestone.

---

## Description

ScavLib is a foundational developer-focused API and utility library designed to
simplify mod development for **Scav Prototype**. It streamlines common tasks such
as building in-game menus, managing configurations, registering console commands,
handling custom events, interacting safely with the game state, and working with
player state, skills, and items.

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
[BepInDependency("com.kanisuko.scavlib", "0.2.3")]
[BepInPlugin("com.yourname.yourmod", "YourMod", "1.0.0")]
public class YourPlugin : BaseUnityPlugin { ... }
```