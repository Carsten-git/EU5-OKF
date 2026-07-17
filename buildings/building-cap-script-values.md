---
type: Playbook
title: Building cap script values
description: Named script_values for max_levels and event auto-fill — REPLACE vanilla caps or add mod-only caps.
tags: [buildings, script-values, caps, events]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Building level limits are **named script_values**, not magic numbers on every building.

# Define caps

```txt
REPLACE:rural_building_cap = {
	add = { desc = "BUILDING_LEVEL_BASE" value = 1 }
	add = { desc = "BUILDING_LEVEL_DEVELOPMENT" value = development multiply = 0.1 }
	add = { desc = "BUILDING_LEVEL_POPULATION" value = population multiply = 0.05 }
	if = {
		limit = { has_river = yes }
		add = { desc = "BUILDING_LEVEL_HAS_RIVER" value = 1 }
	}
}

farming_village_cap = {   # mod-only key — no REPLACE
	add = { desc = "BUILDING_LEVEL_BASE" value = 4 }
	# …
}
```

Gate exotic caps with scripted triggers (`multiply = 0` when disallowed).

# Wire to buildings

```txt
farming_village = {
	max_levels = farming_village_cap
}
```

Inline conditional bonus:

```txt
max_levels = {
	value = rural_building_cap
	if = {
		limit = { owner ?= { has_variable = some_flag } }
		add = { value = 2 }
	}
}
```

# Use in events

```txt
change_building_level_in_location = {
	building = building_type:farming_village
	value = {
		add = num_pop_type:peasants
		divide = 2.5
		max = farming_village_cap
		floor = yes
	}
}
```

Maintenance to cap: `value = cap - location_building_level(...)` with `floor = yes`.

# Related

* [RGO substitution](/buildings/rgo-to-building-substitution.md)
* [Startup world surgery](/total-conversion/startup-world-surgery.md)

# Citations

[1] MnT `in_game/common/script_values/MnT_building_caps.txt`
[2] MnT `in_game/common/building_types/rural_buildings.txt`
[3] MnT `in_game/events/MnT_food.txt`, `MnT_tribes.txt`
[4] Vanilla `in_game/common/script_values/building_caps.txt`
