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

- Runs once per new game, before country selection. It does not run when a save loads. The wiki [On action](https://eu5.paradoxwikis.com/On_action) row for `on_game_start` says it runs before country selection, so `is_ai` is false until a delayed child action. The Community Mod Framework documents a separate `on_game_load` that fires on every save load and does not fire on a new game, which is only needed because vanilla `on_game_start` does not cover loads.
- A save created before the mod was added is not rewritten by this hook. Use a [country pulse](/on-actions/country-pulses.md) only when an old save must be patched.
- Tag-scoped setup (`c:XIU`, `c:HEL`) does not need the player to have picked a country yet.

# See also

* [On-actions overview](on-actions-overview.md)
* [Triggering events](/events/triggering-events.md)
* [Scripted effect basics](/scripted-effects/scripted-effect-basics.md)

# Citations

[1] Vanilla: `game/in_game/common/on_action/_hardcoded.txt` — base `on_game_start` effect block
[2] Vanilla: `game/in_game/common/on_action/ai_personalities_setup.txt` — chained `on_actions` pattern
[3] Mod: `northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt` — mod `on_game_start` handler
[4] Wiki: [On action](https://eu5.paradoxwikis.com/On_action) — `on_game_start` runs before country selection
[5] Wiki: [Community Mod Framework](https://eu5.paradoxwikis.com/Community_Mod_Framework) — `on_game_load` is the save-load hook and does not fire on a new game
