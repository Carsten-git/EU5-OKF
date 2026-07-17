---
type: Reference
title: Triggering events
description: Use trigger_event_non_silently and trigger_event_silently — not trigger_event.
resource: game/in_game/events/situations/western_schism.txt
tags: [events, on-actions, pitfall]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Valid effects (EU5)

Vanilla grep shows **no** bare `trigger_event = { … }`. Use:

| Effect | Use when |
|--------|----------|
| `trigger_event_non_silently` | Player should see popup |
| `trigger_event_silently` | Background chain, AI, delayed follow-ups |

# Syntax forms

**Short form** (common in on-actions):

```txt
trigger_event_non_silently = flavor_teu_nc_purpose.100
```

**Block form** (with delay):

```txt
trigger_event_silently = { id = flavor_teu_nc_tannenberg.2 days = 30 }
```

```txt
trigger_event_non_silently = { id = flavor_teu_nc_purpose.1 }
```

# Pitfall

Using invalid `trigger_event = { id = … }` causes the effect to **do nothing** — no error popup, no event. Intro events and tier notifications simply never appear.

# Example: on game start

```txt
teu_nc_on_game_start = {
	effect = {
		every_country = {
			limit = { tag = TEU is_human = yes NOT = { has_variable = teu_nc_purpose_guide_v3_seen } }
			trigger_event_non_silently = flavor_teu_nc_purpose.100
		}
	}
}
```

# See also

* [On game start](/on-actions/on-game-start.md)
* [Country pulses](/on-actions/country-pulses.md)

# Citations

[1] Vanilla: `in_game/events/situations/western_schism.txt`, `in_game/common/situations/guelphs_and_ghibellines.txt`
