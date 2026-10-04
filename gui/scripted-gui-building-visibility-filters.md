---
type: Playbook
title: Scripted GUI building visibility filters
description: Hide hundreds of building types in production UI using global variable lists, player toggles, and scripted_guis IsBuildingShown gates.
tags: [gui, scripted-gui, buildings, rgo, ui-filters]
timestamp: 2026-07-20T21:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

Mods that replace RGOs with **one building type per good** (50+) cannot leave all types visible in the production lateral view. Deleting building defs breaks saves; **filtering visibility** is the scalable approach.

MnT implements a three-layer stack: global hidden list, player preference vars, and `scripted_guis` referenced from GUI `IsBuildingShown` hooks.

# Layer 1 — Global hidden-building list

At game start (`hide_buildings_on_start` in `MnT_pulse.txt`), call `mnt_global_hide_building` for each RGO-replacement building type:

```txt
mnt_global_hide_building = {
	add_to_global_variable_list = {
		name = mnt_global_hidden_buildings
		target = $building_type$
	}
}
```

`mnt_global_show_building` removes from the same list. See `in_game/common/scripted_effects/MnT_hide_RGOs.txt`.

# Layer 2 — Player toggle vars

| Variable | Meaning |
|----------|---------|
| `mnt_show_rgo_buildings` | User wants RGO-category buildings visible |
| `mnt_force_show_rgo_category` | Transient: show only hidden-list types |
| `mnt_force_show_non_rgo_category` | Transient: show only non-hidden types |

Scripted GUIs in `MnT_toggle_rgo_buildings_visibility.txt` are the **single source of truth** for ON/OFF/toggle; goods-view actions should call these, not duplicate logic.

# Layer 3 — `mnt_is_building_shown` scripted GUI

`scope = building_type` with `saved_scopes = { player }`. Logic (simplified):

1. If `mnt_force_show_rgo_category` → show only types **in** `mnt_global_hidden_buildings`.
2. If `mnt_force_show_non_rgo_category` → show only types **not in** the list.
3. Default: if `mnt_show_rgo_buildings` → show hidden-list types; else hide them.

Wire from `.gui` via `IsBuildingShown` / scripted GUI execute pattern (see [Custom UI patterns](/gui/custom-ui-patterns.md)).

# Design rules

- **Do not** delete building types to reduce UI clutter.
- **Do** populate the global list once at start; use list ops for dynamic show/hide.
- **Clear** transient force-show vars when setting the main ON/OFF state (MnT clears all three filter modes on toggle).
- **Scope** toggles to `is_ai = no` — AI does not need production-view filters.
- Pair with [RGO to building substitution](/buildings/rgo-to-building-substitution.md) for the migration half of the problem.

# Related

* [Mass opt-in / opt-out location flags](/gui/mass-opt-in-opt-out-location-flags.md) — location-level bulk flags (Construction Manager)
* [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) — if patching Glorp production panels

# Citations

[1] MnT-EU5 `in_game/common/scripted_effects/MnT_hide_RGOs.txt`
[2] MnT-EU5 `in_game/common/scripted_guis/MnT_is_building_shown.txt`
[3] MnT-EU5 `in_game/common/scripted_guis/MnT_toggle_rgo_buildings_visibility.txt`
[4] MnT-EU5 `in_game/common/on_action/MnT_pulse.txt` — `hide_buildings_on_start`
