---
type: Reference
title: On game start
description: Hooking mod logic when a new campaign loads via on_game_start on_actions.
resource: game/in_game/common/on_action/_hardcoded.txt
tags: [on-actions, game-start]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Pattern

`in_game/common/on_action/your_mod.txt`:

```txt
on_game_start = {
	on_actions = { teu_nc_on_game_start }
}

teu_nc_on_game_start = {
	effect = {
		c:TEU = { teu_nc_init_purpose_for_country = yes }
		every_country = {
			limit = { tag = TEU is_human = yes … }
			trigger_event_non_silently = flavor_teu_nc_purpose.100
		}
	}
}
```

Vanilla `on_game_start` in `_hardcoded.txt` uses a direct `effect = { … }`; **mods extend** via `on_actions = { … }` chained handlers (see `ai_personalities_setup.txt`).

# Limits

- Runs once per new game — not on save load.
- Use [country pulses](/on-actions/country-pulses.md) as fallback for migrated saves.

# See also

* [On-actions overview](on-actions-overview.md)
* [Triggering events](/events/triggering-events.md)
* [Scripted effect basics](/scripted-effects/scripted-effect-basics.md)

# Citations

[1] Vanilla: `game/in_game/common/on_action/_hardcoded.txt` — base `on_game_start` effect block
[2] Vanilla: `game/in_game/common/on_action/ai_personalities_setup.txt` — chained `on_actions` pattern
[3] Mod: `northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt` — mod `on_game_start` handler
