---
type: Reference
title: game_data category
description: The game_data category field on static modifiers — country, location, province, and scope pairing.
resource: game/main_menu/common/static_modifiers/00_modifier_types.txt
tags: [modifiers, game_data, scope]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Custom static modifiers and modifier **type definitions** include a `game_data` block with a `category` field. Category tells the engine which scope the modifier applies to and where it appears in UI tooling.

# Syntax

On a **custom static modifier** (`main_menu/common/static_modifiers/`):

```txt
teu_nc_purpose_meter = {
	game_data = { category = country }
	monthly_prestige = 0.001
}
```

On a **type definition** (`modifier_type_definitions/00_modifier_types.txt`):

```txt
discipline = {
	decimals = 2
	game_data = {
		category = country
	}
}
```

# Valid categories

From the header comment in `00_modifier_types.txt`:

| Category | Applied to | Typical apply effect |
|----------|------------|----------------------|
| `country` | Countries | `add_country_modifier` |
| `location` | Locations (map tiles) | `add_location_modifier` |
| `province` | Provinces | `add_province_modifier` |
| `character` | Characters | `add_character_modifier` |
| `unit` | Military units | Unit modifier hooks |
| `mercenary` | Mercenary companies | Mercenary-specific |
| `religion` | Religions | Religion-scoped defs |
| `internationalorganization` | IOs | League/HRE-style orgs |
| `rebel` | Rebel factions | Rebel modifiers |
| `dynasty` | Dynasties | Dynasty bonuses |
| `all` | Default on type defs | Engine-wide stat registration |
| `none` | Hidden / non-display | Special cases |

Most mod flavor work uses **`category = country`** for meters, discipline bonuses, and estate tweaks.

# Scope pairing rules

Applying a modifier with the wrong effect fails silently or never stacks:

| Definition category | Use this effect |
|--------------------|-----------------|
| `country` | `add_country_modifier = { modifier = id … }` |
| `location` | Location-scoped add/remove effects |
| `province` | Province-scoped effects |

Stat keys inside the block must also be registered with a compatible category in `modifier_type_definitions`. A `location`-category key like `local_devastation_recovery` on a `category = country` modifier will not behave as intended.

# Vanilla examples

**Country base** — `main_menu/common/static_modifiers/country.txt`:

```txt
country_base_values = {
	game_data = {
		category = country
	}
	…
}
```

**Location base** — `main_menu/common/static_modifiers/location.txt`:

```txt
location_base_values = {
	game_data = {
		category = location
	}
	local_devastation_recovery = 0.005
	…
}
```

# UI visibility

Type definitions may set `should_show_in_modifiers_tab = no` inside `game_data` to hide engine internals. Custom display modifiers for player-facing meters should use visible stats and loc — see [Displaying hidden mechanics](displaying-hidden-mechanics.md).

# Mod checklist

- [ ] Every custom static modifier has `game_data = { category = … }`
- [ ] Category matches the `add_*_modifier` effect you use in script
- [ ] Stat keys inside belong to the same category in type definitions
- [ ] Loc uses `STATIC_MODIFIER_*` keys for custom ids

# See also

* [Static modifiers](static-modifiers.md)
* [Modifier stat keys](modifier-stat-keys.md)
* [Displaying hidden mechanics](displaying-hidden-mechanics.md)

# Citations

[1] `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt` — category comment block
[2] `northern_crusade_teu/main_menu/common/static_modifiers/teu_nc_modifiers.txt`
[3] `game/main_menu/common/static_modifiers/location.txt`
