---
type: Playbook
title: Temporary script-spawned buildings
description: Spawn/destroy temporary buildings from effects — instant vs map-marker construction, short build + long standing duration, without leftovers.
tags: [buildings, construct_building, destroy_building, remove_if]
timestamp: 2026-07-12T11:00:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.1.2"
---

Temporary buildings (projects, worksites) leak easily if a pulse both completes and re-spawns in the same tick.

# Choose spawn mode

| Goal | Spawn | Progress UI |
|------|-------|-------------|
| Building exists immediately (employment, etc.) | `instant = yes` | No map construction ring |
| Expand-RGO-style map pie ring for full duration | **omit** `instant`; long `build_time` | [Construction map markers](/buildings/construction-map-markers.md) Pattern A |
| Brief map ring, then long employment / goods PM | **omit** `instant`; **`build_time ≈ 1`** | Ring during raise only; duration via month counter + standing building |

Always include `cost_multiplier_reason` when setting `cost_multiplier` (even `0`) — [KI-062](/validation/known-issues.md).

# Pattern A — instant (no map ring)

```txt
construct_building = {
	building_type = building_type:my_project
	cost_multiplier = 0
	cost_multiplier_reason = "game_concept_event"
	instant = yes
}
```

Drive duration with a month counter / timed modifier if needed. Timed chips: [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md).

# Pattern B — construction queue equals project length

See [Construction map markers](/buildings/construction-map-markers.md) Pattern A. Complete on `has_building` only when construction length **is** the project. Re-queue with `num_civil_constructions < 1` if cancelled.

# Pattern C — short construct, long standing worksite (recommended for tools/employment)

1. `build_time = 1` (or similarly short).
2. On start: set `months_left`, optional timed `in_progress` modifier, `construct_building` (no `instant`).
3. While busy: monthly decrement; `ensure_project` only if finished building missing and queue empty.
4. Complete when `months_left <= 1` — **never** solely because `has_building` became true ([KI-073](/validation/known-issues.md)).
5. Finished building’s `unique_production_methods` create real market demand (e.g. tools) for the standing duration.

# Pulse hygiene (all patterns)

Never spawn **after** complete in the same loop:

```txt
if = {
	limit = { /* finished? */ }
	my_complete = yes          # destroys building, clears busy
}
else = {
	my_ensure_project = yes    # only while still running
}
```

# Safety net

```txt
remove_if = {
	location = {
		OR = {
			NOT = { exists = var:project_busy }
			NOT = { var:project_busy = 1 }
		}
	}
}
```

Destroy: `destroy_building = "building(building_type:…|owner)"` from location scope.

# See also

* [KI-062](/validation/known-issues.md) — `cost_multiplier_reason`
* [KI-064](/validation/known-issues.md) — respawn-after-complete
* [KI-066](/validation/known-issues.md) — instant hides map marker
* [KI-073](/validation/known-issues.md) — premature complete on `has_building`

# Citations

[1] RGO Conversion — respawn-after-complete; short build + month-counter worksite
[2] Vanilla `destroy_building = "building(building_type:…|owner)"`
