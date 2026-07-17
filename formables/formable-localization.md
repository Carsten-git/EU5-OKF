---
type: Reference
title: Formable localization
description: Loc keys for formable countries — titles, descriptions, allow tooltips, and UTF-8 BOM requirements.
tags: [formables, localization, tooltips]
timestamp: 2026-07-06T08:42:00+10:00
resource: game/main_menu/localization/english/formable_countries_l_english.yml
status: draft
---

Formable localization lives under `main_menu/localization/<language>/`. Vanilla uses `formable_countries_l_english.yml`; mods may add keys to a flavor file (e.g. `teu_nc_l_english.yml`).

**Remember:** EU5 loc files require **UTF-8 with BOM** — see [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md).

# Required keys per formable

For formable ID `ODR_f`:

| Key | Purpose | Example |
|-----|---------|---------|
| `ODR_f` | Form button / list title | `"Form Orderstaat"` |
| `ODR_f_desc` | Description in form country panel | Flavor paragraph |
| `ODR` | Country name (via `name = ODR` in definition) | `"Orderstaat"` |
| `ODR_ADJ` | Adjective (via `adjective = ODR_ADJ`) | `"Orderstaat"` |

Vanilla pattern:

```yml
l_english:
 PRU_f: "Prussia"
 PRU_f_desc: "The land once conquered and Germanized by the Teutonic Order will always require protection..."
```

Mod pattern (`northern_crusade_teu`):

```yml
 ODR_f: "Form Orderstaat"
 ODR_f_desc: "The Teutonic Order survives Grünwald and becomes a permanent military state..."
 ODR: "Orderstaat"
 ODR_ADJ: "Orderstaat"
```

Some vanilla formables reuse country names: `ENG_f: "$ENG$"`.

# Allow-block tooltips

Use dedicated keys for `custom_tooltip` text in `allow` and scripted triggers:

```yml
 teu_nc_can_form_odr_tt: "Form the #Y Orderstaat#! — a permanent military crusader state."
 teu_nc_can_form_odr_tannenberg_tt: "Has won a #G decisive victory#! at Tannenberg."
 teu_nc_can_form_odr_purpose_tt: "Purpose of the Order is at least #Y Steadfast#! (50+)."
 teu_nc_can_form_odr_reform_tt: "Has enacted the #Y Centralized Command#! reform."
```

Each `custom_tooltip` block in triggers should map to one loc key so failed requirements show line-by-line in the UI.

# Shared UI strings (vanilla)

Engine strings — usually no mod override needed:

| Key file | Examples |
|----------|----------|
| `economy_l_english.yml` | `FORM_COUNTRY`, `FORM_COUNTRY_TITLE`, location progress |
| `game_rules_l_english.yml` | `rule_ahistorical_formable_countries`, plausible/historical settings |
| `triggers_l_english.yml` | `IS_REQUIRED_FOR_FORMABLE_TRIGGER` |

# Optional flavor keys

Vanilla adds extra keys for complex formables (e.g. `GBR_f_trigger_eng` for subject/destroy requirements). Follow `{FORMID}_trigger_*` or `{FORMID}_not_*` naming when splitting allow reasons into readable lines.

Event option tooltips can reference formables without duplicating `_f` keys:

```yml
 flavor_teu_nc_tannenberg.30.a.tt: "Unlocks the #G Orderstaat#! formable once you enact #Y Centralized Command#!..."
```

# File placement checklist

1. Create or extend `main_menu/localization/english/<mod>_l_english.yml`.
2. Add `ODR_f`, `ODR_f_desc`, country `ODR` / `ODR_ADJ` if new tags.
3. Add every `custom_tooltip` `text =` key used in formable `allow` and scripted triggers.
4. Save with UTF-8 BOM.
5. Reference reform names with loc keys (`teu_nc_centralized_command`) so `#Y Centralized Command#!` in tooltips matches the government panel.

# See also

* [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)
* [Formable triggers and effects](formable-triggers-and-effects.md)
* [Government reforms](/governments/government-reforms.md) — reform loc keys (`reform_key`, `reform_key_desc`)

# Citations

[1] Vanilla: `game/main_menu/localization/english/formable_countries_l_english.yml` — `PRU_f`, `PRU_f_desc`, `ENG_f: "$ENG$"`
[2] Mod: `mod/northern_crusade_teu/main_menu/localization/english/formable_countries/teu_nc_formables_l_english.yml` — `ODR_f`, allow tooltip keys
[3] Mod: `mod/northern_crusade_teu/main_menu/localization/english/teu_nc_l_english.yml` — shared mod flavor keys
[4] Vanilla: `game/main_menu/localization/english/economy_l_english.yml` — `FORM_COUNTRY`, location progress strings
[5] Vanilla: `game/main_menu/localization/english/game_rules_l_english.yml` — formable game-rule labels
[6] Vanilla: `game/main_menu/localization/english/triggers_l_english.yml` — `IS_REQUIRED_FOR_FORMABLE_TRIGGER`
[7] OKF: [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)
