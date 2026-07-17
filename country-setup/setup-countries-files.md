---
type: Reference
title: Setup countries files
description: in_game/setup/countries/ metadata vs main_menu/setup/start/ bookmark state.
resource: game/in_game/setup/countries/
tags: [country-setup, setup, bookmarks]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

EU5 splits country setup across two layers. Mods that only add events or mechanics usually **do not** touch these; total conversions and new bookmarks do.

# Layer 1: Country definitions

**Path:** `in_game/setup/countries/<region>.txt`

**Purpose:** Tag registry and UI metadata — colors, culture/religion definitions, difficulty, unit colors.

**Template** (`00_readme.info`):

```txt
TAG = {
	color = hsv360 { 360 100 100 }
	color2 = hsv360 { 0 0 0 }
	male_regnal_names = {}
	female_regnal_names = {}
	description_category = administrative
	difficulty = 2
}
```

Files are grouped by region (`baltics.txt`, `poland.txt`, `_default.txt` for minor tags). TEU lives in `baltics.txt` alongside `LIV`, `KUR`, etc.

# Layer 2: Bookmark start state

**Path:** `main_menu/setup/start/*.txt` (numbered files)

**Purpose:** Concrete 1337 (or other start) world state.

| File | Contents |
|------|----------|
| `10_countries.txt` | Owned locations, cores, capital, government |
| `05_characters.txt` | Rulers, heirs, `tag = TEU` blocks |
| `16_wars.txt` | Active wars (`caller = TEU`) |
| `26_ai_personalities.txt` | Per-tag AI personality |
| `12_diplomacy.txt`, `20_rivals.txt`, … | Relations |

TEU in `10_countries.txt` lists `own_control_core`, `own_control_integrated`, and `own_control_conquered` location keys — the actual map footprint at campaign start.

# What mods typically change

| Goal | Touch |
|------|-------|
| Flavor events, meters, missions | **No** setup edit — use `tag = TEU`, on-actions, variables |
| New formable tag already in vanilla | May only need loc + [country history](/country-setup/country-tags-and-history.md) |
| Brand-new country tag | Add to `setup/countries/`, then bookmark ownership in `10_countries.txt` |
| Change TEU starting borders | Override locations in modded `10_countries.txt` (high conflict risk) |

Northern Crusade (`northern_crusade_teu`) adds **no** new setup/countries entries; it layers script on vanilla TEU and formable tags.

# `in_game` vs `main_menu`

| Folder | Loaded for |
|--------|------------|
| `in_game/setup/countries/` | Game rules / tag DB |
| `main_menu/setup/start/` | Scenario initialization when a bookmark loads |

Both can be overridden in a mod's mirrored paths under `in_game/` and `main_menu/`.

# Validation

- Location keys in `10_countries.txt` must exist in the map database — typos fail silently or log errors.
- Every tag in start files should have a block in some `setup/countries/` file.
- After setup edits, start a **new campaign**; saves keep old borders.

# See also

* [Country tags and history](country-tags-and-history.md)
* [On game start](/on-actions/on-game-start.md)
* [Mod folder structure](/getting-started/mod-folder-structure.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)

# Citations

[1] Vanilla: `in_game/setup/countries/00_readme.info`, `baltics.txt`
[2] Vanilla: `main_menu/setup/start/10_countries.txt`, `26_ai_personalities.txt`
