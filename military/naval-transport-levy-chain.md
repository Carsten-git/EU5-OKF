---
type: Playbook
title: Naval transport levy chain
description: Split buildable ships from levy units via copy_from, levy defs, and INJECT unlock_levy on advances.
tags: [levies, navy, advances, units, inject]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Vanilla transport advances unlock **buildable** ships (`unlock_unit`). To add age-appropriate **levied** merchant transports without duplicating ship stats, chain three files.

# Chain

```mermaid
flowchart LR
  A[unit_types levy copy_from] --> B[levies allow + unit]
  B --> C[INJECT advance unlock_levy]
  C --> D[loc alias to vanilla names]
```

# 1. Levy-only unit types

```txt
n_cog_levy = {
	category = navy_transport
	copy_from = n_age_1_traditions_transport
	buildable = no
	levy = yes
	age = "age_1_traditions"
}
```

Repeat per age template (`n_age_2_renaissance_transport`, …).

# 2. Levy definitions

```txt
levy_cog = {
	size = 0.002
	allow = {
		pop_type = pop_type:burghers
		location ?= { has_building_with_at_least_one_level = wharf }
	}
	allow_as_crew = {
		OR = {
			pop_type = pop_type:peasants
			pop_type = pop_type:laborers
			pop_type = pop_type:soldiers
		}
	}
	unit = n_cog_levy
}
```

**Implicit rules (MnT comments):** naval levies are pop-evaluated; if crew pop ≠ evaluated pop, crew pool caps ships; same pop type cannot serve multiple crews; burghers cannot be both `allow` and `allow_as_crew`.

Override vanilla naval levies with `REPLACE:levy_fishing_boat` when needed.

# 3. Advance unlocks

```txt
INJECT:unlock_cog_advance = {
	unlock_levy = levy_cog
}
```

Do not redefine the whole advance — vanilla already has `unlock_unit = n_cog`.

# 4. Localization

Alias levy unit names to vanilla: `n_cog_levy: "$n_cog$"`.

# Related

* [Override ladder](/total-conversion/three-root-and-override-ladder.md)
* [Tribal levies](/map/tribal-levies-and-demographics.md)

# Citations

[1] MnT `in_game/common/unit_types/MnT_navy_transport_levies.txt`
[2] MnT `in_game/common/levies/MnT_traditions_levies_navy.txt`
[3] MnT `in_game/common/advances/MnT_2_ship_unlocks.txt`
[4] Vanilla `in_game/common/advances/2_ship_unlocks.txt`
