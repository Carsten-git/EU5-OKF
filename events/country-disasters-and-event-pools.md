---
type: Playbook
title: Country Disasters and their event pools
description: Use a visible country Disaster for sustained severe pressure; keep routine historical flavor in ordinary DHEs.
resource: game/in_game/common/disasters/byzantine_succession_crisis.txt
tags: [events, disasters, dhe, vanilla-pattern]
timestamp: 2026-09-30T18:09:00+10:00
status: complete
---

# Disaster, Situation, or ordinary event?

EU5 distinguishes three surfaces:

- **DHE / country event:** one historical occurrence. Appropriate for routine flavor, a short buff or debuff, or one real decision.
- **Country Disaster:** sustained internal pressure on one country, with a visible ongoing modifier and an event pool.
- **Situation:** a larger strategic system involving countries, membership, phases, resolutions, map presentation, or international interaction.

Do not build a custom hidden score merely because several events share a theme. If the pressure should be visible and severe, use a Disaster. If an occurrence only needs “five percent for five years,” use a normal event and timed modifier.

# Definition shape

Vanilla `byzantine_succession_crisis` demonstrates the full lifecycle:

```txt
my_country_disaster = {
	content_priority = 800
	image = "gfx/interface/illustrations/disaster/example.dds"
	monthly_spawn_chance = monthly_spawn_chance_very_high
	fire_only_once = yes

	can_start = {
		# country and live-state triggers
	}

	can_end = {
		# recovery or categorical outcome trigger
	}

	modifier = {
		# ongoing country modifiers
	}

	on_start = {
		trigger_event_non_silently = my_disaster.1
	}

	on_monthly = {
		random_list = {
			80 = {}
			10 = { trigger_event_silently = my_disaster.2 }
			10 = { trigger_event_silently = my_disaster.3 }
		}
	}

	on_end = {
		trigger_event_non_silently = my_disaster.100
	}
}
```

The scopes documented by vanilla's `common/disasters/readme.txt` are:

- `root` = affected country;
- `scope:disaster` = disaster type.

# Event files

Disaster events remain normal country events in `in_game/events/disaster/`, but use:

```txt
type = country_event
category = disaster_event
```

Each event still has ordinary `trigger`, `immediate`, and `option` blocks. The Byzantine pool checks live conditions such as estate satisfaction, pop satisfaction, prosperity, civil war, money, characters, and active Disaster type.

# Event order and progress

The event pool is normally **non-linear**. `on_monthly` selects a candidate from a weighted random list, then that event's own trigger decides whether its scene is currently valid. An opening event and an end notification may be fixed without turning the middle into a railroaded chain.

There is no generic Disaster progress field. Vanilla uses several models:

- Byzantine Succession Crisis and Crisis of the Sayfawa Dynasty end from restored live state.
- War of the Roses ends from rebels, war, legitimacy, and claimant viability.
- Court and Country imposes a ten-year minimum, live-state outcomes, and a twenty-year maximum.
- Turmoil in Brandenburg and Decline of Majapahit add bespoke visible scores for their specific mechanics.

Do not add a score merely to make the Disaster “progress.” Improving the ordinary values in `can_end` is progression when those values express the crisis.

# Prefer live state over invented route meters

A Disaster can start, select events, expose options, and end from existing values:

- stability and legitimacy;
- prosperity and control;
- food and market conditions;
- manpower and war state;
- estate or pop satisfaction;
- subject liberty desire and opinion;
- ownership, country existence, and diplomacy;
- active visible timed modifiers.

Use variables only for facts the game cannot reconstruct:

- an event has already occurred recently;
- a specific expedition happened;
- one mutually exclusive terminal outcome was selected;
- a temporary target scope must survive.

Do not turn narrative adjectives such as “coercion,” “resilience,” or “conciliation” into numeric counters when ordinary state already represents the consequence.

# Routine flavor around a Disaster

Keep positive or everyday historical color outside the Disaster:

```txt
option = {
	name = my_flavor.1.a
	location:target_city = {
		add_location_modifier = {
			modifier = my_five_year_trade_boom
			years = 5
			mode = replace
		}
	}
}
```

A one-option event is valid. The option confirms the report and makes the effect visible; it does not imply that the player chose whether history happened.

# Repeatable vs continuous Disasters

`fire_only_once` is a Disaster schema field. Vanilla practice is:

- `yes` — one continuous occurrence ending permanently;
- omit the field for repeatable Disasters such as Horde Civil War, generic Succession Crisis, Coup Attempt, and Decline of Empire.

The schema accepts `no`, but omission is the directly evidenced vanilla pattern. Use recurrence when the same internal crisis can genuinely subside and return. Add a timed post-end cooldown so one bad monthly tick does not immediately restart it, add event cooldown facts so monthly-pool events do not repeat immediately, and clear only facts that should not survive the end.

Use one continuous occurrence when the crisis is understood as a single unresolved constitutional or dynastic problem. Keep its ongoing modifier mild enough for the maximum plausible duration.

# Pitfalls

- Do not call a country Disaster a Situation in design files; they are different engine systems.
- Do not use `trigger_event =`; use `trigger_event_non_silently` or `trigger_event_silently`.
- Do not fire severe events from `on_monthly` without their own live-state triggers.
- Include an empty random-list branch so an event does not fire every month.
- Do not make ordinary flavor depend on the Disaster unless the scene is actually part of the crisis.
- Do not keep a Disaster active for generations after its live conditions have recovered merely to wait for a dated finale; use a separate terminal DHE when appropriate.
- Validate historical-tag predicates such as `has_or_had_tag` in Disaster scope before relying on post-formable continuity.

# Related

* [Dynamic historical events](/events/dynamic-historical-events.md)
* [Event triggers and options](/events/event-triggers-and-options.md)
* [Triggering events](/events/triggering-events.md)
* [Hidden effects and AI chance](/events/hidden-effects-and-ai-chance.md)

# Citations

[1] Vanilla `in_game/common/disasters/readme.txt` — Disaster schema and scopes.
[2] Vanilla `in_game/common/disasters/byzantine_succession_crisis.txt` — complete lifecycle and monthly pool.
[3] Vanilla `in_game/events/disaster/byzantine_succession_crisis.txt` — `category = disaster_event`, live-state triggers, options, and cooldown facts.
[4] Vanilla `in_game/common/disasters/horde_civil_war.txt` — repeatable Disaster and occurrence counter.
[5] Vanilla `in_game/common/disasters/court_and_country.txt` plus `common/scripted_triggers/disaster_triggers.txt` — bounded 10–20 year state challenge.
