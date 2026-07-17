---
type: Reference
title: Country-scoped custom loc
description: type = country customizable localization — tier labels, country history, and modifier tooltips.
resource: mod/northern_crusade_teu/in_game/common/customizable_localization/teu_nc_purpose.txt
tags: [customizable-localization, country, localization]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# `type = country`

When `type = country`, triggers inside each `text` block evaluate in **country scope** — the same scope as `ROOT` in country modifier tooltips and many event strings.

# Mod example: Purpose tier name

`northern_crusade_teu/in_game/common/customizable_localization/teu_nc_purpose.txt`:

```txt
teu_nc_purpose_tier_name = {
	type = country

	text = {
		trigger = { has_variable = teu_nc_purpose_zealous }
		localization_key = teu_nc_purpose_tier_zealous
	}
	text = {
		trigger = { has_variable = teu_nc_purpose_steadfast }
		localization_key = teu_nc_purpose_tier_steadfast
	}
	text = {
		trigger = { has_variable = teu_nc_purpose_questioned }
		localization_key = teu_nc_purpose_tier_questioned
	}
	text = {
		trigger = { has_variable = teu_nc_purpose_crisis }
		localization_key = teu_nc_purpose_tier_crisis
	}
	text = {
		localization_key = teu_nc_purpose_tier_unknown
		fallback = yes
	}
}
```

Tier variables are set monthly by `teu_nc_apply_purpose_tier_flags` ([scripted effects](/scripted-effects/variables-and-monthly-mechanics.md)). The custom loc reads those flags; it does not compute tiers from the numeric meter.

# Consumption in static modifier loc

```yaml
 STATIC_MODIFIER_NAME_teu_nc_purpose_meter: "Purpose: [ROOT.GetVariable('teu_nc_order_purpose').GetValue|0]/100 ([ROOT.Custom('teu_nc_purpose_tier_name')])"
```

`ROOT.Custom('teu_nc_purpose_tier_name')` resolves at tooltip time so the tier label stays in sync with flags.

# Vanilla example: country history

`in_game/common/customizable_localization/country_history.txt` maps tags to long intro text:

```txt
country_history = {
	type = country
	text = { localization_key = country_history_TEU trigger = { tag = TEU } }
	…
}
```

Loc body in `main_menu/localization/english/country_history_l_english.yml`:

```yaml
 country_history_TEU: "The glorious #italic Order of Brothers…"
```

Uses `GetCountry('TEU')`, `ShowAreaName`, and character getters — a heavier pattern than tier labels, but the same `type = country` + `tag =` trigger shape.

# Vanilla example: event factions

`GetHaCWFaction` in `01_customizable_event_loc.txt` picks a faction name from `has_variable` checks — useful when event desc needs branching prose without duplicating whole events.

# Design tips

- Keep **one custom loc key per UI slot** (tier name, faction label, etc.).
- Mirror variable flag names from your [scripted triggers](/scripted-triggers/common-trigger-patterns.md) so loc and mechanics stay aligned.
- Always provide a `fallback` entry for saves mid-migration or missing init.
- Prefix mod keys (`teu_nc_`) to avoid clashing with vanilla `country_history_*` keys.

# See also

* [Customizable localization basics](customizable-localization-basics.md)
* [Dynamic text in loc](/localization/dynamic-text-in-loc.md)
* [Advance and mission localization](/localization/advance-and-mission-localization.md)

# Citations

[1] Mod: `northern_crusade_teu/in_game/common/customizable_localization/teu_nc_purpose.txt`
[2] Vanilla: `in_game/common/customizable_localization/country_history.txt`
