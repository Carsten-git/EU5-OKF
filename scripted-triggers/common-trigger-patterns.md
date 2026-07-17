---
type: Reference
title: Common trigger patterns
description: Tag checks, variables, has_advance, has_reform, and scoped country references in EU5 triggers.
resource: mod/northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt
tags: [scripted-triggers, tags, variables, advances, reforms]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

Patterns that appear constantly in vanilla `flavor_teu.txt` and mod `teu_nc_triggers.txt`.

# Tag checks

| Pattern | Meaning | Example |
|---------|---------|---------|
| `tag = TEU` | Current country is TEU | Mission `enabled`, pulse `trigger` |
| `NOT = { tag = PAP }` | Exclude a tag | Event `trigger` |
| `has_or_had_tag = TEU` | Current or former tag | Advance `potential` |
| `country_exists = c:POL` | Another country exists | `flavor_teu.1` trigger |
| `c:POL = { … }` | Scoped checks on Poland | Neighbor conditions |
| `is_neighbor_of = c:POL` | Diplomatic geography | DHE triggers |

Vanilla `flavor_teu.1`:

```txt
trigger = {
	is_subject = no
	country_exists = c:POL
	is_neighbor_of = c:POL
	c:POL = {
		num_of_non_rural >= 10
	}
	government_type = government_type:theocracy
}
```

Mod pulses gate on tag **or** persistent variable so formables keep working after tag change:

```txt
trigger = {
	OR = {
		tag = TEU
		tag = ODR
		has_variable = teu_nc_order_purpose
	}
}
```

# Variables as flags and meters

| Pattern | Use |
|---------|-----|
| `has_variable = teu_nc_purpose_zealous` | Tier flag set by monthly effect |
| `NOT = { has_variable = teu_nc_purpose_guide_v3_seen }` | One-shot intro gate |
| `has_variable = teu_nc_tannenberg_victory` | Event outcome memory |

Tier flags are refreshed in [variables and monthly mechanics](/scripted-effects/variables-and-monthly-mechanics.md); triggers only **read** them.

Combine with `OR` / `AND` for cumulative tiers:

```txt
teu_nc_has_steadfast_purpose = {
	OR = {
		has_variable = teu_nc_purpose_steadfast
		has_variable = teu_nc_purpose_zealous
	}
}
```

# `has_advance`

Checks whether the country has researched an advance:

```txt
has_advance = gunpowder_advance
has_advance = feudalism_advance
```

Vanilla: `institution_triggers.txt`. Mod advances use the same pattern in `teu_nc_check_advance_rewards` effects and mission gates.

Pair with `potential = { has_or_had_tag = TEU }` on the advance definition so only relevant countries see the node. See [Starting technology level](/advances/starting-technology-level.md) so new advances are not pre-unlocked.

# `has_reform`

Government reform unlock checks:

```txt
has_reform = government_reform:teu_nc_centralized_command
has_reform = government_reform:military_order_reform
```

Vanilla `country_triggers.txt` uses `has_reform` inside `country_can_ennoble_trigger`. Mod formable `teu_nc_can_form_odr` requires `government_reform:teu_nc_centralized_command`.

Reforms are defined in `in_game/common/government_reforms/`. See [Reforms and types](/governments/reforms-and-types.md).

# `custom_tooltip` vs `custom_description`

Inside triggers, `custom_tooltip = { text = my_key … }` shows a requirement line in UI (formables, missions).

**Pitfall (vanilla readme):** do not use the same key for the scripted trigger and `custom_description` text — use `my_trigger_text` for the loc key.

# Putting it together

Northern Crusade formable gate:

```txt
teu_nc_can_form_hpr = {
	religion = religion:catholic
	teu_nc_has_steadfast_purpose = yes
	OR = {
		has_variable = teu_nc_completed_counter_reformation_branch
		has_variable = teu_nc_refused_secularization
	}
	opinion = {
		target = c:PAP
		value >= 150
	}
}
```

# See also

* [Scripted trigger basics](scripted-trigger-basics.md)
* [Event triggers and options](/events/event-triggers-and-options.md)
* [Country-scoped custom loc](/customizable-localization/country-scoped-custom-loc.md) — `has_variable` in loc triggers

# Citations

[1] Vanilla: `in_game/events/DHE/flavor_teu.txt`, `in_game/common/scripted_triggers/country_triggers.txt`
[2] Mod: `northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt`
