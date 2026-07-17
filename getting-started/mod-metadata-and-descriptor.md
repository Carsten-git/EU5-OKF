---
type: Reference
title: Mod metadata and descriptor
description: EU5 launcher registration via .metadata/metadata.json — fields, thumbnail, dependencies.
resource: mod/northern_crusade_teu/.metadata/metadata.json
tags: [getting-started, metadata, launcher]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

EU5 uses **JSON metadata** inside a hidden `.metadata/` folder at the mod root. Legacy `.mod` descriptor files from older Paradox titles are not the primary registration path.

# Required layout

```
Documents/Paradox Interactive/Europa Universalis V/mod/your_mod/
├── .metadata/
│   ├── metadata.json       ← required for launcher
│   └── thumbnail.png       ← optional but recommended (512×512, <1 MB)
├── in_game/
├── main_menu/
└── loading_screen/         ← optional
```

The launcher discovers mods by scanning `mod/` subfolders that contain valid metadata. Missing metadata produces `Mod metadata read error` in [error.log](/validation/error-log-debugging.md).

# metadata.json fields

Example from `northern_crusade_teu`:

```json
{
  "name": "The Northern Crusade",
  "id": "northern_crusade_teu",
  "version": "0.3.0-beta",
  "supported_game_version": "1.*",
  "short_description": "An alternative-history Teutonic Order campaign…",
  "tags": [
    "Events",
    "Missions and Decisions",
    "Historical",
    "Gameplay"
  ],
  "relationships": [],
  "game_custom_data": {}
}
```

| Field | Purpose |
|-------|---------|
| `name` | Display name in launcher and Steam upload |
| `id` | Stable machine id — use folder-safe slug; never change after release |
| `version` | Semver string for your mod |
| `supported_game_version` | Wildcard game version (`1.*`, `1.0.*`) |
| `short_description` | Blurb for launcher / Workshop |
| `tags` | Launcher/Steam categories |
| `relationships` | Dependencies on other mods or DLC |
| `game_custom_data.replace_paths` | Paths to strip from vanilla load (advanced) |

# Dependencies

To require another mod:

```json
"relationships": [
  {
    "rel_type": "dependency",
    "id": "other_mod_id",
    "display_name": "Other Mod Name",
    "resource_type": "mod",
    "version": "1.0.*"
  }
]
```

Load order in the launcher playset still matters — dependent mod should sit **below** its dependency so overrides apply correctly.

For the shared **Community Mod Framework** (menus, alerts, hooks), use `id`: `community_mod_framework`, `version`: `2.*`, and also mark it required on Steam. Full how-to: [CMF overview and dependency](/community-mod-framework/overview-and-dependency.md).

# Thumbnail

Place `thumbnail.png` beside `metadata.json`. Used in launcher and Steam Workshop listing. Wiki recommends 512×512 pixels.

# Creating a new mod

Options:

1. **Launcher** — "Create mod" flow scaffolds folder + metadata (per [Modding wiki](https://eu5.paradoxwikis.com/Modding))
2. **Community mod toolkit** — template with placeholder fields ([Mod Template wiki](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-toolkit/wiki/Mod-Template))
3. **Manual** — copy `.metadata/` from an existing mod and edit `id` + `name`

# Enabling in launcher

1. Mod folder under `Documents/…/mod/`
2. Valid `metadata.json`
3. Enable in playset; order relative to dependencies
4. Full restart after structural changes — see [Enabling your mod](enabling-your-mod.md)

# Relationship to .mod files

Some tooling still mentions `descriptor.mod` alongside metadata. EU5 launcher reads **metadata.json** first. If you ship both, keep `id` and paths consistent. This bundle treats `.metadata/metadata.json` as authoritative.

# See also

* [Mod folder structure](mod-folder-structure.md)
* [Enabling your mod](enabling-your-mod.md)
* [Paradox wiki and tools](/references/paradox-wiki-and-tools.md)
* [Error log debugging](/validation/error-log-debugging.md)

# Citations

[1] `northern_crusade_teu/.metadata/metadata.json`
[2] [EU5 Mod structure wiki](https://eu5.paradoxwikis.com/Mod_structure)
