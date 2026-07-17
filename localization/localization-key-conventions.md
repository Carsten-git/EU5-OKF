---
type: Reference
title: Localization key conventions
description: Prefixes, suffixes, and pairing rules for EU5 mod localization keys.
resource: game/main_menu/localization/english/
tags: [localization, naming, conventions]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

Quick reference for consistent, grep-friendly keys. Northern Crusade uses the `teu_nc_` prefix; vanilla TEU flavor uses `flavor_teu` namespaces.

# Mod prefix

Use a short unique prefix on **new** keys (`teu_nc_`, `mymod_`) to avoid overwriting vanilla:

| Good | Risky |
|------|-------|
| `teu_nc_purpose_change_tt` | `purpose_change_tt` |
| `flavor_teu_nc_purpose.100.title` | `purpose.100.title` |

# By content type

| Type | Key shape | Example |
|------|-----------|---------|
| Event title/desc/option | `<namespace>.<id>.title` / `.desc` / `.a` | `flavor_teu.1.title` |
| DHE browser | `<namespace>.<id>.entry` | `flavor_teu_nc_purpose.100.entry` |
| Static modifier name | `STATIC_MODIFIER_NAME_<id>` | `STATIC_MODIFIER_NAME_teu_nc_purpose_meter` |
| Static modifier desc | `STATIC_MODIFIER_DESC_<id>` | `STATIC_MODIFIER_DESC_teu_nc_purpose_meter` |
| Advance | `<advance_id>` / `<advance_id>_desc` | `teu_nc_last_crusade_desc` |
| Mission tree | `<tree_id>` + `_DESCRIPTION` variants | `generic_conquer_province_BUTTON_TOOLTIP` |
| Mission task | `<task_id>` / `<task_id>_desc` / `_tip` | `mission_war_chest_desc` |
| Country history | `country_history_<TAG>` | `country_history_TEU` |
| Custom loc leaf | plain key referenced by `localization_key` | `teu_nc_purpose_tier_zealous` |
| Tooltip only | `_tt` suffix | `teu_nc_can_form_odr_purpose_tt` |

# Event namespace vs loc key

Script:

```txt
namespace = flavor_teu_nc_purpose
flavor_teu_nc_purpose.100 = { title = flavor_teu_nc_purpose.100.title … }
```

Loc uses the **full** event id in keys, not abbreviated. See [Event ID rules](/events/event-id-rules.md) — avoid `.0` ids.

# File naming

| Script file | Loc file |
|-------------|----------|
| `flavor_teu_nc_purpose.txt` | `flavor_teu_nc_purpose_l_english.yml` |
| `teu_nc_l_english.yml` | Shared mod strings (modifiers, tooltips) |

One yml per event script file — [Event localization naming](event-localization-naming.md).

# Formatting tokens

| Token | Use |
|-------|-----|
| `#G` / `#R` / `#Y` | Green / red / yellow highlight |
| `#italic … #!` | Italic span |
| `[ROOT.GetVariable('x').GetValue\|0]` | Numeric interpolation |
| `[ROOT.Custom('key')]` | [Customizable localization](/customizable-localization/) |
| `$other_key$` | Substitute another loc key |

# YML structure

```yaml
l_english:
 my_key: "English text"
```

- Leading space before keys is vanilla style.
- File must be UTF-8 **with BOM** — [UTF-8 BOM requirement](utf8-bom-requirement.md).

# See also

* [Advance and mission localization](advance-and-mission-localization.md)
* [Static modifier localization](static-modifier-localization.md)
* [Dynamic text in loc](dynamic-text-in-loc.md)

# Citations

[1] Mod: `northern_crusade_teu/main_menu/localization/english/teu_nc_l_english.yml`
[2] Vanilla: `main_menu/localization/english/events/DHE/flavor_teu_l_english.yml`
