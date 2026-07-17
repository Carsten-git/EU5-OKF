---
type: Reference
title: On-actions overview
description: How on_actions chain, fire events and effects, and how mods hook pulses without editing vanilla.
resource: game/in_game/common/on_action/ai_personalities_setup.txt
tags: [on-actions, hooks, chaining]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# What on_actions are

Named hooks in `in_game/common/on_action/*.txt`. The engine fires them at game start, on pulses (monthly/yearly per country), on deaths, etc. Each entry can run triggers, effects, events, and **chain** to more on_actions.

Reference spec: `on_actions.info` in the vanilla folder.

# Chaining (mod pattern)

Vanilla `_hardcoded.txt` defines root hooks like `on_game_start` and `monthly_country_pulse`. **Mods append** handlers — they do not replace the root block:

```txt
on_game_start = {
	on_actions = { teu_nc_on_game_start }
}

teu_nc_on_game_start = {
	effect = {
		c:TEU = { teu_nc_init_purpose_for_country = yes }
		every_country = {
			limit = {
				tag = TEU
				is_human = yes
				NOT = { has_variable = teu_nc_purpose_guide_v3_seen }
			}
			trigger_event_non_silently = flavor_teu_nc_purpose.100
		}
	}
}
```

Same pattern as vanilla `ai_personalities_setup.txt`:

```txt
on_game_start = {
	on_actions = { on_ai_personalities_game_start }
}
```

Multiple mod files can each add `on_actions = { my_handler }` to the same root hook; the engine merges lists.

# Handler anatomy

```txt
my_monthly_handler = {
	trigger = { tag = TEU }          # optional — skip if false
	weight_multiplier = { … }        # for random_on_action lists only

	effect = { … }                   # script effects (concurrent with events)

	events = { … }                   # always try these event IDs
	random_events = { … }            # weighted random pick
	first_valid = { … }              # first passing trigger

	on_actions = { child_handler }   # chain deeper
	random_on_action = { … }
	fallback = another_on_action     # if nothing ran
}
```

**Important (`on_actions.info`):** `effect` runs **concurrently** with events fired in the same handler — not before. Do not set a variable in `effect` and assume an event fired in the same handler sees it.

# Country pulses

| Root hook | Scope | Mod example |
|-----------|-------|-------------|
| `monthly_country_pulse` | Each country, each month | `teu_nc_monthly_purpose_pulse` |
| `yearly_country_pulse` | Each country, each year | Save-game Purpose bootstrap |

See [Country pulses](country-pulses.md) and [On game start](on-game-start.md).

Northern Crusade `teu_nc_purpose.txt` registers three roots:

1. `on_game_start` — init + intro event
2. `yearly_country_pulse` — missed-init safety net
3. `monthly_country_pulse` — meter, tier flags, advance rewards

# Events from on_actions

Vanilla `country_monthly.txt` lists fixed `events = { flavor_pap.9000 … }` plus `random_events` pools. Mods usually prefer **explicit effects** + `trigger_event_*` for controlled flavor.

When using `events = { }`, each event still needs its own `trigger` block to pass.

# File organization

- One mod feature per file (`teu_nc_purpose.txt`) keeps merge conflicts low.
- Name handlers uniquely (`teu_nc_*`) to avoid colliding with vanilla or other mods.
- Gate expensive work with `trigger = { … }` so most countries skip your handler.

# See also

* [On game start](on-game-start.md)
* [Country pulses](country-pulses.md)
* [Triggering events](/events/triggering-events.md)
* [Scripted effect basics](/scripted-effects/scripted-effect-basics.md)

# Citations

[1] Vanilla: `in_game/common/on_action/on_actions.info`, `country_monthly.txt`
[2] Mod: `northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt`
