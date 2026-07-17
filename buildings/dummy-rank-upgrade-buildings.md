---
type: Playbook
title: Dummy rank-upgrade buildings
description: Automate rural→town / town→city by constructing a 1-level dummy building whose on_built delays an on_action that changes location rank and removes the building.
tags: [buildings, location-rank, automation, on-actions]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

Construction Manager pattern **CM-6**: location rank changes need a **constructible hook** so automation and GUI cost/can-build bindings apply. CM ships dummy building types that exist only to trigger rank change.

# Problem

Script-only `change_location_rank` bypasses construction cost / queue UX. Players and automations expect urbanize to behave like a build.

# Pattern

1. Define `building_types` with max level 1, custom icon, appropriate allow triggers.
2. `on_built` → `trigger_event_silently` / delayed `on_action` (CM uses `days = 1`).
3. On-action: if dummy building present → decrement/remove it → `change_location_rank`.

```txt
# building_types (conceptual)
on_built = {
	location = {
		trigger_event_silently = {
			on_action = cm_upgrade_location_rank_on_action
			days = 1
		}
	}
}
```

```txt
cm_upgrade_location_rank_on_action = {
	effect = {
		if = {
			limit = { has_building = building_type:cm_upgrade_town_to_city_building }
			change_building_level_in_location = {
				building = building_type:cm_upgrade_town_to_city_building
				value = -1
			}
			if = {
				limit = { location_rank = location_rank:town }
				change_location_rank = location_rank:city
			}
		}
		# rural → town analogous
	}
}
```

# Automation integration

Stage the dummy building type into the same [construction queue](/gui/offscreen-scripted-widget-drivers.md) as other builds (`cm_stage_urbanize_candidate`). Eligibility triggers enforce pops, ignore-lists, discounts, and conflicts (e.g. auto-food locking a location with `cannot_upgrade_location`).

# Companion modifier

CM applies `cm_auto_food_locked_location` (`cannot_upgrade_location = yes`) when auto-food owns a location so urbanize does not undo food management — product-specific but shows **modifier locks** as a clean gate.

# Related

* [Temporary script-spawned buildings](/buildings/temporary-script-buildings.md)
* [Construction map markers](/buildings/construction-map-markers.md)
* [CMM priority feature dispatcher](/community-mod-framework/cmm-priority-feature-dispatcher.md)

# Citations

[1] CM `in_game/common/building_types/cm_location_upgrade_buildings.txt`
[2] CM `in_game/common/on_action/cm_on_location_rank_upgrade.txt`
[3] CM `main_menu/common/static_modifiers/cm_location.txt` — lock modifier
