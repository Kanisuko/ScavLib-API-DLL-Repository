# ScavLib — Scav Prototype Mod API Library

**Version:** 0.7.2

**Required by:** Any mod built on ScavLib  

**Nexus Mods:** [Nexus Mods Page](https://www.nexusmods.com/scavprototype/mods/83)  

**Source Code:** [Kanisuko/ScavLib-API](https://github.com/Kanisuko/ScavLib-API)  

---

## Description

ScavLib is a foundational developer-focused API and utility library designed to
simplify mod development for **Scav Prototype** (a.k.a. **Casualties Unknown**). It streamlines common tasks such
as building in-game menus, managing configurations, registering console commands,
handling custom events, interacting safely with the game state, and working with
player state, skills, limbs, and items.

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
   to the console. Mods registered with a lifecycle object are annotated with `[F]`.

---

## Requirements

- **BepInEx 5.x** (64-bit version recommended)
- Casualties Unknown game client (on Steam)

---

## For Developers

Full API documentation is available on the [Wiki](https://github.com/Kanisuko/ScavLib-API-DLL-Repository/wiki).
All public methods carry XML doc comments visible in any IDE with IntelliSense support.