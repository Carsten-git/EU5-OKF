---
type: Playbook
title: Construction map markers
description: How to get an Expand-RGO-style pie-ring progress marker on the map for scripted projects — and when not to tie completion to it.
tags: [buildings, construction, map-markers, gui, rgo]
timestamp: 2026-07-12T11:00:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.1.2"
---

Vanilla **Expand RGO** progress on the map is a **civil `Construction`**, not a timed location modifier.

# Engine pieces

| Piece | Path / API |
|-------|------------|
| Map widget | `in_game/gui/map_markers_construction.gui` → `name = "building_construction_marker"` |
| Progress | `Construction.GetProgress` / `GetInverseProgress` |
| Icon | `GetConstructionIcon(Construction.Self)` (RGO uses goods icon when `Construction.IsRGO`) |
| Settings category | Map marker settings → Building Construction |

Related (no progress ring): `raw_goods_marker` in `map_markers.gui` — static current RGO icon only.

# How to get the ring for a mod project

1. Define a `building_type` with `build_time = <days>` (script value OK).
2. `construct_building = { building_type = … cost_multiplier = 0 cost_multiplier_reason = "…" }` — **omit `instant = yes`**.
3. The ring tracks **construction only**. It ends when the building finishes.

```txt
# Start (location scope)
construct_building = {
	building_type = building_type:my_project
	cost_multiplier = 0
	cost_multiplier_reason = "game_concept_event"
}
```

# Completion models (pick one)

| Goal | Construction | Complete when |
|------|--------------|---------------|
| A. Progress = full project length | `build_time` ≈ project days | `has_building` (monthly) — map ring matches duration |
| B. Short raise, then long standing worksite (employment / goods PM) | `build_time` ≈ **1** day | Month counter / timed modifier — **not** `has_building` alone |

**Pattern B** (RGO Conversion): brief pie ring → finished building runs tools PM + employment for `rgo_conversion_duration_months`; monthly tick decrements `months_left` and only then calls complete.

```txt
# Monthly (Pattern B — while busy)
if = {
	limit = { var:months_left <= 1 }
	my_complete = yes
}
else = {
	change_variable = { name = months_left subtract = 1 }
	my_ensure_project = yes   # only while still busy
}
```

# What does **not** create a map ring

* `add_location_modifier` / `Location.GetTimedModifiers` chips (location panel only)
* `construct_building` with **`instant = yes`**
* Scripted GUI alone (no new engine marker types without cabinet/construction piggyback)
* Custom mapmodes filtered by `has_location_modifier` (no vanilla “modifier X” mapmode)

# Duration caveats

* `build_time` is base days; construction capacity can finish faster/slower (same as Expand RGO).
* For a **fixed calendar length with employment/PM after finish**, use Pattern B — do not set `build_time` to the full duration and also complete on the first `has_building` pulse (see [KI-073](/validation/known-issues.md)).
* Pie ring will **not** show remaining standing-worksite time after construction ends; use a timed location modifier chip for that.

# See also

* [Temporary script-spawned buildings](/buildings/temporary-script-buildings.md)
* [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md)
* [KI-066](/validation/known-issues.md), [KI-073](/validation/known-issues.md)

# Citations

[1] Vanilla `in_game/gui/map_markers_construction.gui` — `building_construction_marker`
[2] Vanilla `in_game/gui/shared/production_tooltips.gui` — `Construction_tooltip`, `Construction.IsRGO`
[3] RGO Conversion — short `build_time` + month counter for tools/employment duration
