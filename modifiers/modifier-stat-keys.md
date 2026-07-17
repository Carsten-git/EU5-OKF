---
type: Reference
title: Modifier stat keys
description: Finding valid modifier and advance bonus keys from vanilla modifier_type_definitions.
resource: game/main_menu/common/static_modifiers/00_modifier_types.txt
tags: [modifiers, validation, syntax]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Static modifiers, advances, government reforms, and missions can all apply **modifier stat keys** as `key = value` lines. Keys must be registered in vanilla type definitions or they silently do nothing.

# Authoritative source

```
game/main_menu/common/modifier_type_definitions/00_modifier_types.txt
```

Each entry defines one stat:

```txt
discipline={
	decimals=2
	game_data={
		category=country
	}
}
```

The left-hand id (`discipline`) is the key you use in advances and static modifiers.

# Discovery workflow

**1. Grep vanilla usage** — find a working example before inventing a key:

```powershell
rg "global_pop_conversion_speed_modifier" "$env:GAME_ROOT" --glob "*.txt"
```

**2. Confirm key exists** in `00_modifier_types.txt`:

```powershell
rg "^discipline\s*=" "$env:GAME_ROOT\main_menu\common\modifier_type_definitions"
```

**3. Check category** — country advances/modifiers need `category=country` on the type (most military/economy stats are country-scoped). See [game_data category](game-data-category.md).

**4. Automate** — `validate_mod.py` loads all keys from `00_modifier_types.txt` and flags unknown ids in your mod files. See [Mod validation tooling](/validation/mod-validation-tooling.md).

# Where keys appear

| Content type | Example location |
|--------------|------------------|
| Static modifiers | `main_menu/common/static_modifiers/your_mod.txt` |
| Advances | `in_game/common/advances/*.txt` |
| Government reforms | `in_game/common/government_reforms/*.txt` |
| Mission `modifier_while_progressing` | `in_game/common/missions/*.txt` |

Advances share the same key namespace as static modifiers. Skip structural advance fields when validating — see [Advance triggers and modifiers](/advances/advance-triggers-and-modifiers.md).

# Value shapes

| Value type | Example | Notes |
|------------|---------|-------|
| Decimal bonus | `discipline = 0.05` | Most common |
| Integer | `fort_limit = 1`, `cultures_capacity = 2` | Check type definition |
| Boolean | `may_explore = yes` | Often unlock flags, not modifier types |
| Named tier | `clergy_estate_target_satisfaction = medium_permanent_target_satisfaction` | Grep vanilla for valid tier tokens |
| Societal value | `monthly_towards_centralization = 0.1` | Special family; validator whitelists prefix |

# Common mistakes

| Mistake | Symptom |
|---------|---------|
| Typo in key name | No bonus; often no log line |
| EU4 key name | Key absent from EU5 definitions |
| Location key on country modifier | Wrong `game_data` category on custom static modifier |
| Duplicate conflicting keys | Last writer wins — hard to debug |

# Related vanilla files

| File | Role |
|------|------|
| `main_menu/common/static_modifiers/country.txt` | Examples of country stat application |
| `main_menu/localization/english/modifiers_l_english.yml` | Display names for modifier types |
| `main_menu/localization/english/static_modifiers_l_english.yml` | Custom static modifier names |

# See also

* [Static modifiers](static-modifiers.md)
* [game_data category](game-data-category.md)
* [Advance triggers and modifiers](/advances/advance-triggers-and-modifiers.md)
* [Common pitfalls](/validation/common-pitfalls.md)

# Citations

[1] `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt`
[2] `northern_crusade_teu/tools/validate_mod.py` — `load_modifier_types()`
