---
type: Reference
title: Static modifiers
description: Defining teu_nc_* style country modifiers and applying them in script.
resource: mod/northern_crusade_teu/main_menu/common/static_modifiers/teu_nc_modifiers.txt
tags: [modifiers, static_modifiers]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

# Definition

`main_menu/common/static_modifiers/teu_nc_modifiers.txt`:

```txt
teu_nc_purpose_meter = {
	game_data = { category = country }
	monthly_prestige = 0.001
}

teu_nc_purpose_zealous = {
	game_data = { category = country }
	discipline = 0.025
	monthly_religious_influence = 0.05
}
```

# Applying

```txt
add_country_modifier = { modifier = teu_nc_purpose_meter years = -1 mode = add_and_extend }
remove_country_modifier = teu_nc_purpose_meter
```

`years = -1` with `mode = add_and_extend` is a common pattern for "permanent until removed" display modifiers refreshed monthly.

# Localization

[Static modifier localization](/localization/static-modifier-localization.md) — keys must match modifier id.

# Validation

Unknown modifier stat keys fail silently — validate against [modifier stat keys](modifier-stat-keys.md) or `validate_mod.py`.

# See also

* [Modifier stat keys](modifier-stat-keys.md)
* [game_data category](game-data-category.md)
* [Displaying hidden mechanics](displaying-hidden-mechanics.md)

# Citations

[1] [Mod static modifiers](mod/northern_crusade_teu/main_menu/common/static_modifiers/teu_nc_modifiers.txt)
[2] [Vanilla modifier type registry](game/main_menu/common/static_modifiers/00_modifier_types.txt)
[3] [Static modifier localization](/localization/static-modifier-localization.md)
