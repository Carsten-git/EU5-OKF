---
type: Playbook
title: Custom modifier type registration
description: End-to-end contract for new modifier keys — definitions, icons, loc, and consumers across main_menu and in_game.
tags: [modifiers, modifier-types, localization, icons]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

A new modifier key is a **five-file contract**. Missing any piece → silent tooltip/icon/scope failures.

# Contract

| Step | Root | Path |
|------|------|------|
| 1. Schema/docs (optional) | `main_menu` | `common/modifier_type_definitions/` |
| 2. Live definition | `in_game` | `common/modifier_type_definitions/` |
| 3. Icon | `main_menu` | `common/modifier_icons/` |
| 4. Localization | `main_menu` | `MODIFIER_TYPE_NAME_*` / `MODIFIER_TYPE_DESC_*` |
| 5. Consumers | either | static_modifiers, auto_modifiers, buildings, prices, traits |

# Definition

```txt
build_price_farming_village_cost_modifier = {
	color = bad
	percent = yes
	game_data = {
		category = country
	}
}
```

**Trap (MnT):** the `in_game` modifier_type_definitions **filename must be all-lowercase** or definitions silently fail.

# Icon + loc

```txt
build_price_farming_village_cost_modifier = {
	positive = "gfx/interface/icons/modifier_types/small_rural_building_cost_modifier.dds"
}
```

Loc keys: `MODIFIER_TYPE_NAME_build_price_farming_village_cost_modifier`, `MODIFIER_TYPE_DESC_…`.

# Related

* [game_data category](/modifiers/game-data-category.md)
* [Static modifiers](/modifiers/static-modifiers.md)
* [Modifier stat keys](/modifiers/modifier-stat-keys.md)

# Citations

[1] MnT `in_game/common/modifier_type_definitions/MnT_modifier_types.txt` (note lowercase requirement comment)
[2] MnT `main_menu/common/modifier_type_definitions/mnt_modifier_types.txt`
[3] MnT `main_menu/common/modifier_icons/MnT_modifier_icons.txt`
[4] Vanilla `main_menu/common/modifier_type_definitions/00_modifier_types.txt`
