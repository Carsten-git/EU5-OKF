---
type: Reference
title: Event file basics
description: Structure of a country_event in EU5 — namespace, type, options.
tags: [events, syntax]
timestamp: 2026-07-06T00:00:00+10:00
resource: game/in_game/events/DHE/
status: draft
---

# Minimal country event

```txt
namespace = flavor_teu_nc_purpose

flavor_teu_nc_purpose.100 = {
	type = country_event
	title = flavor_teu_nc_purpose.100.title
	desc = flavor_teu_nc_purpose.100.desc
	outcome = neutral

	immediate = {
		set_variable = { name = teu_nc_purpose_guide_v3_seen value = 1 }
	}

	option = {
		name = flavor_teu_nc_purpose.100.a
	}
}
```

# Common fields

| Field | Purpose |
|-------|---------|
| `type` | Usually `country_event` |
| `title` / `desc` | Loc keys |
| `outcome` | `good`, `bad`, `neutral` — UI tone |
| `fire_only_once` | With DHE or manual firing |
| `trigger` | Conditions when event can fire |
| `immediate` | Effects on fire, before options |
| `option` | Player/AI choices |

# File location

Flavor country events: `in_game/events/DHE/flavor_<TAG>.txt` (vanilla pattern). Mods may add parallel files e.g. `flavor_teu_nc_purpose.txt`.

# See also

* [Event ID rules](event-id-rules.md)
* [Event localization naming](/localization/event-localization-naming.md)

# Citations

[1] Vanilla: `game/in_game/events/DHE/flavor_teu.txt` — `country_event` structure, `type`, `title`, `desc`, `outcome`, `immediate`, `option`
[2] Mod: `mod/northern_crusade_teu/in_game/events/DHE/flavor_teu_nc_purpose.txt` — parallel mod flavor file pattern
[3] Vanilla: `game/in_game/events/DHE/` — flavor country events live under `flavor_<TAG>.txt`
