---
type: Playbook
title: Enabling your mod
description: Launcher, load order, and when a full restart is required.
tags: [getting-started, launcher, testing]
timestamp: 2026-07-06T00:00:00+10:00
resource: mod/northern_crusade_teu/.metadata/metadata.json
status: complete
---

# Steps

1. Place the mod folder under `Documents/Paradox Interactive/Europa Universalis V/mod/` with valid [metadata](mod-metadata-and-descriptor.md).
2. Enable it in the EU5 launcher (load order: mod **below** dependencies if you add any).
3. Start a **new game** when testing:
   - advances with changed `starting_technology_level`
   - `on_game_start` hooks
   - one-shot intro events with migration flags
4. For loc-only fixes: quit to desktop and relaunch (hot reload is unreliable for yml).

# Load order

Later mods override earlier ones for the same file path. Document overrides if you patch vanilla TEU events vs add parallel `flavor_teu_nc_*` files.

# See also

* [Smoke testing checklist](/validation/smoke-testing-checklist.md)
* [Common pitfalls](/validation/common-pitfalls.md)

# Citations

[1] Mod: `mod/northern_crusade_teu/.metadata/metadata.json` — launcher descriptor / mod identity
[2] OKF: [Mod metadata and descriptor](mod-metadata-and-descriptor.md) — folder placement under `Documents/Paradox Interactive/Europa Universalis V/mod/`
[3] Vanilla: `game/in_game/common/on_action/_hardcoded.txt` — `on_game_start` (requires new game when hooking start logic)
[4] Mod: `mod/northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt` — `starting_technology_level` changes need new save
[5] OKF: [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md) — loc fixes need full relaunch
