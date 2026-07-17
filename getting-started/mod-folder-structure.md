---
type: Reference
title: Mod folder structure
description: How EU5 mods split content between in_game, main_menu, and loading_screen trees.
resource: game/
tags: [getting-started, structure, folders, loading_screen]
timestamp: 2026-07-11T11:00:00+10:00
eu5_paths: [in_game/, main_menu/, loading_screen/]
status: complete
---

EU5 mods mirror the game's own directory layout. Content under each tree loads only in that lifecycle phase.

# Layout

```
my_mod/
├── .metadata/           # metadata.json + thumbnail
├── in_game/
│   ├── common/          # advances, on_action, scripted_effects, buildings, …
│   ├── events/          # event scripts (often events/DHE/ for country flavor)
│   ├── gui/             # in-session lateral views, shared templates
│   └── …                # map_data, gfx, localization (minimal)
├── main_menu/
│   ├── common/          # static_modifiers, game_rules, game_concepts, …
│   ├── setup/           # bookmark start files
│   ├── gui/             # menu / frontend overrides
│   └── localization/
│       └── english/     # .yml loc files (UTF-8 with BOM) — default home for loc
└── loading_screen/      # optional; total conversions often use this
    ├── common/defines/  # selective define overrides
    └── gui/             # branded loading UI, version label
```

# Rules of thumb

| Put it in… | Examples |
|------------|----------|
| `in_game/` | Events, on-actions, advances, missions, buildings, climates, session GUI |
| `main_menu/` | Static modifiers, **game rules**, localization, setup, game concepts, menu GUI |
| `loading_screen/` | Global `defines` (only changed keys), boot branding |

Localization almost always lives under **`main_menu/localization/<language>/`**, even when the scripted content is in `in_game/`.

For large overhauls, prefer [INJECT / REPLACE / full-copy ladder](/total-conversion/three-root-and-override-ladder.md) over wholesale `replace_path` until you must excise vanilla trees.

# Mod registration

EU5 mods register via `.metadata/metadata.json` in the mod root (not legacy `.mod` alone). See [Mod metadata and descriptor](mod-metadata-and-descriptor.md).

Keep the mod **folder** next to other mods under:

`Documents/Paradox Interactive/Europa Universalis V/mod/`

# See also

* [Mod metadata and descriptor](mod-metadata-and-descriptor.md)
* [Enabling your mod](enabling-your-mod.md)
* [Three-root architecture](/total-conversion/three-root-and-override-ladder.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)
* [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)

# Citations

[1] Vanilla game install: `…/Europa Universalis V/game/in_game/`, `…/game/main_menu/`, `…/game/loading_screen/`
[2] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md) — three-root TC example
