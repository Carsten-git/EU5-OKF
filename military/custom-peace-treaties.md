---
type: Playbook
title: Custom peace treaties
description: Add or REPLACE peace_treaties with location select_trigger, building destruction effects, and AI desire blocks.
tags: [military, peace, diplomacy, war, treaties]
timestamp: 2026-07-20T21:45:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

Peace conference options live in `in_game/common/peace_treaties/`. Mods can add **new treaty types** or **`REPLACE:`** vanilla treaties to change cost, targeting, effects, and AI weighting without touching C++.

MnT's `REPLACE:dismantle_fortifications` is the reference for **location-scoped** military peace terms.

# File location

```
in_game/common/peace_treaties/MnT_dismantle_fort.txt
```

Loc keys for treaty names/descriptions typically go in `main_menu/localization/english/` (see [Main menu event localization mirror](/localization/main-menu-event-localization-mirror.md) for the general main_menu vs in_game split).

# REPLACE pattern

```txt
REPLACE:dismantle_fortifications = {
	category = dismantle_fort

	cost = {
		add = {
			desc = "DIPLOREASON_BASE"
			value = "scope:target.location_peace_cost(scope:loser|scope:winner)"
			multiply = 1
		}
	}

	base_antagonism = 1
	antagonism_type = antagonism_dismantle_fortifications
	are_targets_exclusive = yes

	select_trigger = {
		looking_for_a = location
		source = recipient
		target_flag = target
		visible = {
			has_fort = yes
			NOT = { has_building = building_type:theodosian_walls }
			owner = scope:recipient
		}
	}

	effect = {
		scope:target = {
			every_buildings_in_location = {
				limit = { building_category = defense_category }
				location = { destroy_building = prev }
			}
		}
	}

	ai_desire = {
		if = {
			limit = { scope:target = { is_neighbor_of = scope:winner } }
			add = { desc = "DIPLOREASON_NEARBY_FORT" value = 2 }
		}
		if = {
			limit = { scope:loser = { is_rebel_country = yes } }
			multiply = { desc = "DIPLOREASON_CIVIL_WAR" value = 0 }
		}
	}
}
```

# Techniques

| Piece | Purpose |
|-------|---------|
| `category` | Groups treaty in peace UI (vanilla categories like `dismantle_fort`) |
| `cost` + `location_peace_cost` | Scale diplomatic cost to location value |
| `antagonism_type` | Tie into antagonism system |
| `are_targets_exclusive = yes` | One winner per target location |
| `select_trigger` + `looking_for_a = location` | Player/AI picks **tiles**, not countries |
| `visible` / `allow` | Fort presence, exclusions (wonders), owner checks |
| `effect` + `destroy_building` | Iterate `defense_category` buildings on tile |
| `ai_desire` | Neighbor forts weigh higher; zero in civil wars |

# New treaty vs REPLACE

| Approach | When |
|----------|------|
| `my_dismantle_forts = { … }` | Brand-new peace option; add loc + icon |
| `REPLACE:vanilla_key = { … }` | Patch vanilla behaviour while keeping the same treaty id |

Pair with [Soft-disable vanilla systems](/total-conversion/soft-disable-vanilla-systems.md) when you need to hide vanilla treaties via `potential = { always = no }` instead.

# Related

* [Wargoals and casus belli](/military/) — war entry (thin in bundle; grep vanilla `common/casus_belli/`)
* [REPLACE generic actions](/interactions/replace-generic-actions.md) — similar `REPLACE:` pattern outside peace UI
* [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)

# Citations

[1] MnT-EU5 `in_game/common/peace_treaties/MnT_dismantle_fort.txt`
[2] Vanilla `game/in_game/common/peace_treaties/` — schema reference
