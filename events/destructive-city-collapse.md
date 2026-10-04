---
type: Playbook
title: Destructive city collapse from an event
description: Move a capital, remove completed buildings, reset an RGO to level 1, and apply a long migration penalty with vanilla effects.
resource: game/in_game/events/situations/war_of_religions.txt
tags: [events, locations, capital, buildings, rgo, migration]
timestamp: 2026-09-30T18:09:00+10:00
status: complete
---

# Compose the outcome from narrow effects

For a destructive city collapse:

1. move the capital directly;
2. iterate and destroy completed buildings;
3. subtract all RGO levels above 1;
4. apply existing prosperity/control penalties;
5. apply a visible timed migration modifier.

Do not use a convenience capital-move effect that also transfers development or prosperity unless that transfer is intended.

# Direct capital move

Country scope:

```txt
set_capital = location:new_capital
```

Vanilla uses this direct effect in Hungarian, Habsburg, Serbian, Ethiopian, and other country events.

# Remove completed buildings

Exact vanilla pattern from `war_of_religions.17`:

```txt
location:target_city = {
	every_buildings_in_location = {
		save_temporary_scope_as = target_building
		PREV = {
			destroy_building = scope:target_building
		}
	}
	change_prosperity = prosperity_radical_penalty
}
```

The iterator covers completed building objects. It is not proven to cancel queued construction or remove every protected/unique building with normal `destroy_building`. Smoke-test both cases before claiming “every building” is permanently gone. `destroy_building_forcefully` is registered and has vanilla call sites for specific protected objects, but use it in the iterator only if testing proves it necessary.

# Reset RGO level to 1 without changing the good

Current vanilla `flavor_swe.38`:

```txt
location:target_city = {
	if = {
		limit = {
			rgo_level > 1
		}
		change_max_raw_material_workers = {
			value = rgo_level
			subtract = 1
			multiply = -1
		}
	}
}
```

At level 4 this applies −3 and leaves level 1. Do not call `change_raw_material` when the good must remain.

The Columbian Exchange uses the related `rgo_workers` value plus `min = 0`. Prefer the exact `rgo_level` pattern when the requirement is expressed as displayed RGO level, then verify it in game.

# Long outward migration

Define a visible location modifier:

```txt
my_abandoned_city = {
	game_data = {
		category = location
	}
	local_migration_attraction = -5
}
```

Apply it:

```txt
location:target_city = {
	add_location_modifier = {
		modifier = my_abandoned_city
		years = 50
		mode = replace
	}
}
```

Vanilla's Shadow of Cahokia uses `local_migration_attraction = -5`; vanilla DHEs use exact fifty-year location modifiers.

# Verification

- destination is owned and becomes capital;
- country remains playable;
- all completed source-city buildings are gone;
- protected/unique buildings either disappear normally or use a validated forceful fallback;
- queued construction behavior is known;
- RGO good is unchanged;
- displayed RGO level is exactly 1;
- timed migration modifier expires on the exact date;
- prosperity and control penalties are visible.

# Related

* [Change raw material](/economy/change-raw-material.md)
* [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md)
* [Country Disasters and event pools](/events/country-disasters-and-event-pools.md)

# Citations

[1] Vanilla `in_game/events/situations/war_of_religions.txt`, `war_of_religions.17`.
[2] Vanilla `in_game/events/DHE/flavor_SWE.txt`, `flavor_swe.38`.
[3] Vanilla `in_game/events/DHE/flavor_HUN.txt`.
[4] Vanilla `main_menu/common/static_modifiers/location.txt`, `chk_shadow_of_cahokia_city`.
