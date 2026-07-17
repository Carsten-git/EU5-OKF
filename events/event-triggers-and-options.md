---
type: Reference
title: Event triggers and options
description: Event trigger blocks, immediate effects, options, and historical_option in EU5 country events.
resource: game/in_game/events/DHE/flavor_HUN.txt
tags: [events, triggers, options]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# Trigger block

`trigger = { … }` gates whether the event **can** fire. Evaluated for DHE monthly rolls, `trigger_event_*` calls, and on-action event lists.

Vanilla `flavor_teu.1` (Prussian Confederation):

```txt
trigger = {
	is_subject = no
	country_exists = c:POL
	is_neighbor_of = c:POL
	at_war = no
	government_type = government_type:theocracy
	any_owned_non_rural_location = {
		area = area:prussia_area
	}
}
```

Use [scripted triggers](/scripted-triggers/) for repeated gates:

```txt
trigger = {
	teu_nc_has_crisis_purpose = yes
	NOT = { has_variable = teu_nc_purpose_crisis_notified }
}
```

# `dynamic_historical_event`

DHE metadata is separate from `trigger`:

```txt
dynamic_historical_event = {
	tag = TEU
	from = 1450.1.1
	to = 1500.1.1
	monthly_chance = 5
}
```

See [Dynamic historical events](dynamic-historical-events.md) and [DHE browser visibility](dhe-browser-visibility.md).

# `immediate`

Runs when the event fires, **before** the player sees options. Use for scopes, illustration effects, and one-shot flags.

Mod intro `flavor_teu_nc_purpose.100`:

```txt
immediate = {
	set_variable = { name = teu_nc_purpose_intro_seen value = 1 }
	set_variable = { name = teu_nc_purpose_guide_v3_seen value = 1 }
}
```

Vanilla `flavor_teu.1` creates a rebel and runs `event_illustration_government_estate_effect` in `immediate`.

# Options

Each `option = { … }` is one player/AI choice.

| Field | Role |
|-------|------|
| `name` | Loc key for button text |
| `historical_option = yes` | AI bias + UI marker for "historical" pick |
| `custom_tooltip` | Extra tooltip block (can nest effects) |
| Effects | Prestige, rebels, `trigger_event_*`, etc. |

Vanilla option chaining to Poland:

```txt
option = {
	name = flavor_teu.1.a
	historical_option = yes
	c:POL = { trigger_event_silently = { id = flavor_teu.2 } }
}
```

See [Triggering events](triggering-events.md) for silent vs non-silent follow-ups.

# Dynamic `desc`

Events can branch description text without separate event IDs:

```txt
desc = {
	first_valid = {
		triggered_desc = {
			trigger = { exists = scope:target scope:target ?= { is_alive = yes } }
			desc = flavor_teu.100.desc.crusader_returns
		}
		triggered_desc = {
			trigger = { always = yes }
			desc = flavor_teu.100.desc.fallback
		}
	}
}
```

Pattern from `flavor_teu.100` (End of the Prussian Crusade).

# Trigger vs option effects

| Location | When it runs |
|----------|----------------|
| `trigger` | Before fire — must pass |
| `immediate` | On fire — always if event fired |
| `option` | When that option is chosen |

Do not put one-time notification flags in `trigger` — use `immediate` or the chosen option.

# See also

* [Event file basics](event-file-basics.md)
* [Hidden effects and AI chance](hidden-effects-and-ai-chance.md)
* [Common trigger patterns](/scripted-triggers/common-trigger-patterns.md)
* [Event localization naming](/localization/event-localization-naming.md)

# Citations

[1] Vanilla: `in_game/events/DHE/flavor_teu.txt`
[2] Mod: `northern_crusade_teu/in_game/events/DHE/flavor_teu_nc_purpose.txt`
