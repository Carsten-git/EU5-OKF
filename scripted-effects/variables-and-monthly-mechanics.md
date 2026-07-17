---
type: Playbook
title: Variables and monthly mechanics
description: Pattern for a 0–100 meter with tiers, monthly delta, and UI display.
resource: mod/northern_crusade_teu/in_game/common/scripted_effects/teu_nc_effects.txt
tags: [scripted-effects, variables, modifiers, purpose-pattern]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Variables

| Variable | Role |
|----------|------|
| `teu_nc_order_purpose` | Score 0–100 |
| `teu_nc_purpose_monthly_delta` | Sum of changes this month (reset each pulse) |
| `teu_nc_purpose_zealous` etc. | Tier flags (mutually exclusive via scripted effect) |
| `teu_nc_purpose_guide_v3_seen` | One-shot intro migration |

# Monthly pulse outline

1. Init if missing (`teu_nc_init_purpose_for_country`)
2. `set_variable` monthly delta to `0`
3. Apply gains/losses via `teu_nc_change_purpose = { value = … }`
4. `teu_nc_apply_purpose_tier_flags` → updates flags + [display modifiers](/modifiers/displaying-hidden-mechanics.md)
5. `teu_nc_try_purpose_intro` for human players

# Tier thresholds

```txt
if = { limit = { var:teu_nc_order_purpose >= 80 } set_variable = teu_nc_purpose_zealous … }
else_if = { limit = { var:teu_nc_order_purpose >= 50 } … }
```

Zealous tier can also set `teu_nc_purpose_steadfast` so triggers requiring "Steadfast+" pass.

# UI

- [Customizable localization](/localization/dynamic-text-in-loc.md) for tier name
- Static modifier name shows `ROOT.GetVariable('teu_nc_order_purpose')`

# Reference implementation

`northern_crusade_teu/in_game/common/scripted_effects/teu_nc_effects.txt`

# See also

* [Country pulses](/on-actions/country-pulses.md)
* [Displaying hidden mechanics](/modifiers/displaying-hidden-mechanics.md)

# Citations

[1] Mod: `northern_crusade_teu/in_game/common/scripted_effects/teu_nc_effects.txt` — `teu_nc_init_purpose_for_country`, tier flags, monthly delta
[2] Mod: `northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt` — monthly pulse wiring
[3] Mod: `northern_crusade_teu/in_game/common/customizable_localization/teu_nc_purpose.txt` — tier name display
