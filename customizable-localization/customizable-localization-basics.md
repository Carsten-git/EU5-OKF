---
type: Reference
title: Customizable localization basics
description: Defining runtime loc keys in common/customizable_localization/ and calling them from yml.
resource: game/in_game/common/customizable_localization/
tags: [customizable-localization, localization, syntax]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# Location

`in_game/common/customizable_localization/<mod>.txt`

Reference: `customizable_localization.info` in the vanilla folder.

# Structure

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
		localization_key = teu_nc_purpose_tier_unknown
		fallback = yes
	}
}
```

| Field | Role |
|-------|------|
| `type` | Scope for triggers (`country`, etc.) |
| `text` | One candidate loc key + optional `trigger` |
| `localization_key` | Plain loc key resolved in yml |
| `fallback = yes` | Default if no earlier `text` matches |
| `random_valid = yes` | Pick randomly among valid entries (optional) |

# Evaluation order

The engine walks `text` entries **top to bottom** and uses the first whose `trigger` passes. Put specific conditions first; end with a `fallback` entry without a trigger.

Vanilla `GetHaCWFaction` in `01_customizable_event_loc.txt` follows the same pattern for event dynamic text.

# Parent + suffix variants

```txt
child_key = {
	parent = parent_key
	suffix = "_suffix"
	fallback = true
}
```

Runs parent logic, then appends suffix to the chosen loc key. Useful when many keys share one trigger tree.

# Using in localization

In yml strings, call with scope `.Custom('key')`:

```yaml
 STATIC_MODIFIER_NAME_teu_nc_purpose_meter: "Purpose: [ROOT.GetVariable('teu_nc_order_purpose').GetValue|0]/100 ([ROOT.Custom('teu_nc_purpose_tier_name')])"
```

See [Dynamic text in loc](/localization/dynamic-text-in-loc.md) for variable formatting.

# Loc keys for `localization_key`

Each `localization_key` needs a matching entry in `main_menu/localization/english/`:

```yaml
 teu_nc_purpose_tier_zealous: "Zealous"
 teu_nc_purpose_tier_steadfast: "Steadfast"
```

# See also

* [Country-scoped custom loc](country-scoped-custom-loc.md)
* [Localization key conventions](/localization/localization-key-conventions.md)
* [Displaying hidden mechanics](/modifiers/displaying-hidden-mechanics.md)

# Citations

[1] Vanilla: `in_game/common/customizable_localization/customizable_localization.info`
[2] Mod: `northern_crusade_teu/in_game/common/customizable_localization/teu_nc_purpose.txt`
