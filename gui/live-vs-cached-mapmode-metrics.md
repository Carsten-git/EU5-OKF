---
type: Playbook
title: Live vs cached mapmode metrics
description: Choose script_value-in-map_color (live) vs location-variable cache (one-shot or rare refresh) for custom map modes — performance and correctness trade-offs from Zorange's Mapmode Collection.
tags: [gui, map-modes, script-values, performance, on-actions]
timestamp: 2026-07-13T17:10:00+10:00
status: complete
source_mod: zoranges.mapmode.collection
source_version: "1.0"
---

Utility map-mode mods often color locations by a **computed metric**. Zorange's Mapmode Collection (ZMC, workshop `3697317887`) uses **two** strategies side by side — pick deliberately.

# Problem

`map_color` / tooltips need a number per location. Computing heavy geography or pop loops every paint can hitch; never refreshing a dynamic economy metric lies to the player.

# Strategy A — Live script_value

Call the script value **inside** `map_color` limits / `lerp` factors and in tooltip loc via `MakeScope.ScriptValue('…')`.

```txt
# map_modes excerpt
else_if = {
	limit = { zmc_control_times_market_access >= 1 }
	value = rgb { 135 206 250 }
}
else_if = {
	limit = {
		zmc_control_times_market_access < 1
		zmc_control_times_market_access >= 0.5
	}
	lerp = {
		max_color = rgb { 0 255 0 }
		min_color = rgb { 255 255 0 }
		factor = {
			value = zmc_control_times_market_access
			subtract = 0.5
			divide = 0.5
		}
	}
}
```

**Refresh:** `color_refresh_counters = { Month }` (and matching `color_and_names_refresh_counters`).

**Use when:** metric is cheap (modifiers, control × market access, building-slot %, pop growth) and must track gameplay month-to-month.

**Avoid when:** script value walks `every_pop` or world scans — ZMC’s literacy mode is a cautionary example (loc even notes lag risk).

# Strategy B — Cached location variable

1. Define a **script_value** that scores the location (e.g. topography + vegetation + climate + harbor).
2. On setup, `every_location_in_the_world` → `set_variable = { name = … value = <script_value> }`.
3. `map_color` reads `var:…` (and `NOT = { has_variable = … }` for water / unset).
4. Refresh counters match how often the inputs change — ZMC uses `TopographyVegetationDatabaseUpdate` for urbanisation suitability (not Month).

```txt
# on_action setup (new games + mid-campaign enable)
on_game_start = { on_actions = { zmc_on_game_start } }
monthly_country_pulse = { on_actions = { zmc_on_game_start } }  # until global done

zmc_on_game_start = {
	trigger = { NOT = { has_global_variable = zmc_urbanization_setup_done } }
	effect = {
		set_global_variable = zmc_urbanization_setup_done
		every_location_in_the_world = {
			limit = { is_land = yes }
			set_variable = {
				name = zmc_local_urbanisation_suitability
				value = zmc_local_urbanisation_suitability_level
			}
		}
	}
}
```

**Use when:** score is expensive or static relative to month (terrain, climate, fixed presence tables).

**Mid-campaign mod install:** ZMC’s `monthly_country_pulse` + global “setup done” flag ensures existing saves get the cache once without recomputing forever.

# Banded lerp (both strategies)

Split the scale into bands; remap `factor` inside each band with `subtract` / `divide` so red→yellow→green stays readable. Water / uncolonised / owned get **flat** colors before lerp branches.

# Guardrails

| Issue | Fix (ZMC examples) |
|-------|-------------------|
| Divide by zero | `min = 0.0001` on growth denominators (`zmc_time_until_100_development_sub_value`) |
| Uncolonised | Separate `NOT = { exists = owner }` branch + gray legend |
| Water | `is_land = no` → water HSV / dedicated tooltip |
| Over-cap % | Extra purple/black bands above 100% |

# Related

* [Custom map modes](/gui/custom-map-modes.md) — full file checklist
* [Mapmode localization and game concepts](/gui/mapmode-localization-and-concepts.md)
* [Mod performance pulses and scans](/on-actions/mod-performance-pulses-and-scans.md)
* [Construction Manager map modes](/gui/custom-map-modes.md#construction-manager-worked-example-cm-7) — trigger-driven automation heatmaps

# Citations

[1] Workshop `3697317887` — `zmc_map_modes.txt`, `zmc_on_game_start.txt`, `zmc_*_script_values.txt`
[2] ZMC urbanisation = Strategy B; control×market / building cap / pop = Strategy A
