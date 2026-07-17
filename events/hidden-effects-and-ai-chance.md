---
type: Reference
title: Hidden effects and AI chance
description: hidden_effect in event options and ai_chance weighting for AI option selection.
resource: game/in_game/events/DHE/flavor_teu.txt
tags: [events, ai, effects]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# `hidden_effect`

Effects inside `hidden_effect = { … }` run with the option but **do not** appear in the event tooltip. Use for cleanup, silent character death, or background chains the player should not spoil.

Vanilla `flavor_teu.14` option b:

```txt
option = {
	name = flavor_teu.14.b
	add_prestige = prestige_mild_penalty

	hidden_effect = {
		kill_character_silently = {
			target = scope:target_character
		}
	}
}
```

Same pattern in `flavor_teu.17.b` — visible penalty on the button, death handled quietly.

**Contrast:** visible effects in the option body show in tooltips (`add_prestige`, `change_government_type`, etc.). Put narrative-spoiling or technical follow-up work in `hidden_effect`.

Pair with [triggering events](triggering-events.md): use `trigger_event_silently` inside `hidden_effect` when the follow-up should not announce early.

# `ai_chance`

Each option can define AI pick weighting:

```txt
option = {
	name = flavor_teu.6.a

	ai_chance = {
		factor = 1
		modifier = {
			factor = 0
			gold <= {
				value = building_type:castle.building_base_cost_in_gold
				multiply = 0.75
			}
		}
	}

	scope:target_location = {
		construct_building = {
			building_type = building_type:castle
			cost_multiplier = 0.5
			cost_multiplier_reason = "game_concept_event"
		}
	}
}
```

When `cost_multiplier` is set (including `0`), EU5 **requires** `cost_multiplier_reason = <loc key>` (vanilla often uses `"game_concept_event"`). Missing it → script system error / PostValidate false (KI-062).

| Piece | Meaning |
|-------|---------|
| `factor = 1` | Base weight |
| `modifier = { factor = 0 trigger }` | Zero weight when broke — AI picks other option |
| `historical_option = yes` | Additional AI preference (vanilla DHE) |

`flavor_teu.6` uses opposing `ai_chance` modifiers on options a and b so the AI builds the castle when affordable and takes the unrest option when gold is low.

# Design notes

- Give the AI a **viable** fallback option (`factor = 0` on one side implies the other wins).
- Do not rely on `ai_chance` alone for player-facing balance — humans can always pick the "wrong" option.
- `hidden_effect` does not hide the option itself, only extra effects in the tooltip.

# Mod usage

Northern Crusade onboarding events are player-only (`is_human` gates in on-actions); they omit `ai_chance` because AI TEU does not need Purpose tutorials. Combat and reform events in `flavor_teu_nc_*` can copy vanilla `ai_chance` patterns when AI countries reach the same branches.

# See also

* [Event triggers and options](event-triggers-and-options.md)
* [Triggering events](triggering-events.md)
* [Dynamic historical events](dynamic-historical-events.md)

# Citations

[1] Vanilla: `in_game/events/DHE/flavor_teu.txt` (`flavor_teu.6`, `.14`, `.17`)
