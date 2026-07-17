---
type: Playbook
title: Estate-paid building maintenance
description: Route building upkeep to estates by power using auto_modifiers, caches, and monthly charge events.
tags: [economy, estates, auto-modifiers, epbm]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

EU5 charges building upkeep to the country. To make **estates pay a share by relative power**, MnT's EPBM subsystem shows the workable architecture when there is no native estate-upkeep API.

# Idea

1. Set country `building_upkeep_multiplier` baseline to **full** cost (e.g. `1.0`).
2. Each month, compute what each estate should pay.
3. Apply **negative** `building_upkeep_multiplier` auto_modifiers scaled so the treasury only keeps the crown share.
4. Transfer gold from estates via `add_gold_to_estate` (negative).

# Cost pools

Loop **all three** building iterators the engine exposes:

| Pool | Source | Who pays |
|------|--------|----------|
| Estate-owned | `every_building_owned_by_estate` | 100% that estate |
| Shared | Non-government owned buildings | Split by `estate_power` |
| Crown | `government_category` buildings | Crown / treasury |

# Auto-modifier credit

```txt
# Concept — scales_with a script_value built from estate_power × shared_fraction × inflation
epbm_nobles_upkeep = {
	building_upkeep_multiplier = -1.0
	scales_with = epbm_nobles_upkeep_scale
}
```

Weight shared costs with `estate_power(estate_type:…)`. Fold inflation into the scale (`1 + inflation`) if upkeep should track prices.

# Performance: market PM cache

Summing every building's maintenance goods every month is expensive. Cache per-PM cost on the **market center location** in a `variable_map`, clear monthly. Maps often need remove-before-readd.

# Display lag

UI and charge can be one month apart. Keep separate **show/prev** vars from **calc** vars so tooltips match what was charged.

# Save compatibility

Bump a rebuild version global when the state shape changes; gate monthly hooks until rebuild completes (MnT: `@epbm_rebuild_version` + TTL).

# Related

- [INJECT production methods](/buildings/inject-production-methods.md) — maintenance PM baskets
- [Naming prefixes](/total-conversion/naming-and-subsystem-prefixes.md) — `epbm_` isolation

# Citations

[1] MnT `in_game/common/scripted_effects/epbm_calculate.txt`, `epbm_effects.txt`, `epbm_charge.txt`
[2] MnT `in_game/common/auto_modifiers/epbm_country.txt`
[3] MnT `in_game/events/epbm_events.txt`
[4] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
