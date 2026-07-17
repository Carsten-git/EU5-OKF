---
type: Reference
title: Dynamic historical events
description: The dynamic_historical_event block for time-windowed flavor events.
resource: game/in_game/events/DHE/flavor_HUN.txt
tags: [events, dhe]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Block shape

```txt
flavor_teu_nc_tannenberg.1 = {
	type = country_event
	fire_only_once = yes

	dynamic_historical_event = {
		tag = TEU
		from = 1400.1.1
		to = 1450.12.31
		monthly_chance = 20
	}

	trigger = {
		is_subject = no
		OR = {
			is_at_war_with = c:POL
			is_at_war_with = c:LIT
		}
		owns = location:malbork
		owns = location:konigsberg
	}
	…
}
```

# Semantics

- Listed in the **Dynamic Historical Events** browser for the tag.
- Rolls monthly inside the date window when `trigger` is true.
- `monthly_chance` is a weight, not a percent (compare vanilla `flavor_hun.1`).

# Not every event is DHE

Condition-fired chain events (purpose intro, tier warnings) have **no** `dynamic_historical_event` block — they will not appear in the DHE list.

# See also

* [DHE browser visibility](dhe-browser-visibility.md)
* [Event localization naming](/localization/event-localization-naming.md)

# Citations

[1] Vanilla: `in_game/events/DHE/flavor_HUN.txt`, `flavor_teu.txt`
