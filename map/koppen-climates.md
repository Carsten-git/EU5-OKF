---
type: Playbook
title: Köppen climates
description: Replace coarse vanilla climates with a fine taxonomy and keep old content working via alias triggers.
tags: [map, climates, triggers, location-templates]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Vanilla EU5 ships a small climate set. A historically grounded overhaul needs a **finer taxonomy** (e.g. Köppen-Geiger) plus a migration path for content that still says `tropical` / `arctic`.

# Four deliverables

1. **New climate definitions** — `in_game/common/climates/` with `location_modifier`, `color`, winter rules, colonial hooks.
2. **Bulk reassignment** — every location's `climate = …` in `location_templates` (prefer [CSV pipeline](/map/csv-location-templates-pipeline.md)).
3. **Compatibility triggers** — map old names to OR-blocks of new keys.
4. **Downstream consumers** — diseases, biomes, goods, mapmodes updated to aliases or new keys.

# Compatibility shim

```txt
tropical_climate_trigger = {
	OR = {
		climate = rainforest_climate
		climate = monsoon_climate
		climate = savanna_summer_climate
		climate = savanna_winter_climate
	}
}
```

Diseases and other systems call the trigger instead of a single vanilla climate key.

# QA

- `debug_color` on climates for screenshot mapmodes.
- Balance numbers in a spreadsheet; keep a link/comment in the climate file header.
- Loc + `named_colors` for mapmode labels (Af, Cfb, …).

# Design notes from MnT

- Removed `local_army_attrition` from climates (attrition blocked reinforcement/morale).
- Pair `always_winter` climates with topography for snowy impassables visually.
- Special cases (e.g. Andean tundra) need explicit keys, not just latitude rules.

# Citations

[1] MnT `in_game/common/climates/MnT_default.txt`
[2] MnT `in_game/common/scripted_triggers/MnT_climate_triggers.txt`
[3] MnT `in_game/map_data/location_templates.txt`
[4] [CSV location templates pipeline](/map/csv-location-templates-pipeline.md)
