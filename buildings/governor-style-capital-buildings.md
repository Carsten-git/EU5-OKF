---
type: Reference
title: Governor-style capital buildings
description: Vanilla local-governor eligibility, caps, balance inputs, AI markers, and estate-power reference values.
resource: game/in_game/common/building_types/capital_buildings.txt
tags: [buildings, province-capital, local-governor, ai, estate-power]
timestamp: 2026-10-22T20:50:00+11:00
status: complete
---

# Vanilla local governor baseline

`local_governor` is defined in `in_game/common/building_types/capital_buildings.txt`. It is a one-level noble government building with these settlement flags:

```txt
rural_settlement = no
	town = no
	city = yes
	megalopolis = yes
```

Its `allow` block enforces a country-wide quantity cap through `modifier:num_local_governors`, adjusted by existing lieutenancies. That cap is separate from `max_levels = 1`: `max_levels` limits one location, while the `allow` comparison limits the country's total.

The vanilla `location_potential` requires a road to the country capital and rejects the country capital itself. These are local-governor rules, not universal requirements for all government buildings. A mod building should include or omit them intentionally.

# Province-capital eligibility and cleanup

`is_province_capital = yes` is a verified location trigger for building eligibility. Vanilla `thema_headquarters` uses it directly in an `allow` block. This differs from `is_capital`, which vanilla uses for the country capital.

Building `remove_if` is evaluated with the building as root; vanilla commonly scopes through `location = { ... }` for location conditions. The building-type readme says a true `remove_if` automatically destroys the building. A province-capital-bound building can therefore declare completed-building cleanup with a location-scoped negative `is_province_capital` check.

Static vanilla evidence does **not** establish that `remove_if` also cancels a queued or active building level. If relocation must cancel construction, test that state explicitly and do not treat completed-building removal as proof.

# Cost, time, and maintenance

The vanilla local governor uses:

- `build_time = government_build_time`; vanilla defines this as **365 days**.
- `construction_demand = local_governor_construction`.
- A unique maintenance production method, `local_governor_maintenance`.

Vanilla construction demand:

| Good | Amount |
|------|--------|
| Masonry | 5 |
| Tools | 5 |
| Glass | 5 |
| Fine cloth | 2 |
| Gold | 2 |

Vanilla maintenance inputs:

| Good | Amount |
|------|--------|
| Tools | 1 |
| Masonry | 1 |
| Weaponry | 0.5 |
| Paper | 3 |
| Books | 1 |
| Gold | 1 |
| Furniture | 2 |
| Fine cloth | 1 |

For a half-scale derivative, halve every entry separately and halve the unmodified build time. This yields 182.5 days; whether the game accepts or displays a half-day must be validated before implementation rather than silently rounded in design.

# AI behavior

The building sets `important_for_AI = yes`, `ai_forbid_shutdown = yes`, and `ai_ignore_maintenance = yes`, while `automation_build_allowed = no`. Engine defines also name local-governor-specific AI behavior:

- `AI_LOCAL_GOVERNOR_TAX_BASE_THRESHOLD = 100`
- `AI_PROXIMITY_CANDIDATE_UPDATE_CHANCE = 0.2`
- `AI_CITY_UPGRADE_GOVERNOR_POTENTIAL_FACTOR = 0.4`

Do not assume a custom building automatically receives hard-coded local-governor candidate logic merely by copying visible flags. Verify custom-building AI behavior in vanilla code patterns or an observer test, and record any gap as an unvalidated claim during solution-option work.

# Nobility estate-power scale

The valid location modifier is `local_nobles_estate_power`; it is a percentage-type location stat. Vanilla building definitions use raw numbers, not a named `small` / `medium` / `large` token for this modifier.

Observed positive building values include **0.1**, **0.2**, **0.25**, **0.5**, and **1.0**. The smallest positive vanilla building value is:

```txt
local_estates = {
	modifier = {
		local_nobles_estate_power = 0.1
	}
}
```

Therefore `0.1` is a defensible vanilla-reference “small” value for a location building. This is an empirical scale convention, not a globally declared tier constant.

# Validation checklist

- Distinguish one-per-location (`max_levels`) from a country-wide cap (`allow`).
- Set every intended settlement-rank flag explicitly.
- Use `is_province_capital` for the province capital; do not substitute country-capital `is_capital`.
- If capital loss must remove the building or cancel construction, validate both completed and under-construction states.
- Verify custom-building AI construction in an observer run; local-governor-specific engine logic may not generalize.
- Check fractional build-time behavior if the design produces non-integer days.

# Citations

1. `game/in_game/common/building_types/capital_buildings.txt` — `local_governor`.
2. `game/in_game/common/goods_demand/building_construction_costs.txt` — `local_governor_construction`.
3. `game/main_menu/common/script_values/default_values.txt` — `government_build_time`.
4. `game/loading_screen/common/defines/00_defines.txt` — local-governor AI defines.
5. `game/in_game/common/building_types/estate_buildings.txt` — positive `local_nobles_estate_power` building values.
6. `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt` — `local_nobles_estate_power`.
