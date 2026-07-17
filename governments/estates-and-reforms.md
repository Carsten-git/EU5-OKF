---
type: Reference
title: Estates and reforms
description: How government reforms interact with estates — privileges, power, satisfaction, and formation gates.
tags: [governments, reforms, estates, privileges]
timestamp: 2026-07-06T08:40:00+10:00
resource: game/in_game/common/government_reforms/
status: draft
---

Government reforms frequently **read estate state** in triggers and **write estate behavior** through modifiers. Estates are not defined inside reform files, but reforms are a primary way modders reshape estate power.

# Estate checks in reform definitions

**Locked reforms** tie removal to estate privileges:

```txt
military_order_reform = {
	locked = {
		has_estate_privilege = estate_privilege:clergy_military_orders
	}
	potential = {
		religion = religion:catholic
		has_policy = monastic_order
	}
}
```

While `clergy_military_orders` privilege exists, the military order reform stays locked in place.

# Estate modifiers on reforms

Reforms apply estate-facing modifiers in `country_modifier`:

```txt
country_modifier = {
	clergy_estate_levy_size = 0.25
	clergy_estate_allowed_leading_military = yes
	global_nobles_estate_power = 0.1
	nobles_estate_target_satisfaction = medium_permanent_target_satisfaction
	burghers_estate_target_satisfaction = medium_permanent_target_satisfaction
	global_crown_estate_power = 0.15
	crown_estate_cannot_marry = yes
}
```

Common modifier families:

| Pattern | Example use |
|---------|-------------|
| `*_estate_target_satisfaction` | Permanent satisfaction bias |
| `global_*_estate_power` | Shift balance between estates |
| `*_estate_levy_size` | Military contribution (military orders) |
| `crown_estate_*` | Ruler/central authority rules |
| `government_reform_slots` | Extra reform capacity (papacy, legates) |

Mod reforms follow the same pattern — e.g. `teu_nc_officer_aristocracy` boosts `nobles_estate_levy_size` and `global_crown_estate_power`.

# Estate gates on formables

Formables can require specific privilege and power balances. Vanilla **Prussia** (`PRU_f`):

```txt
allow = {
	NOT = {
		has_reform = government_reform:military_order_reform
		has_estate_privilege = estate_privilege:clergy_powerful_dioceses
	}
	has_estate_privilege = estate_privilege:nobles_land_rights
	"estate_power(estate_type:clergy_estate)" < 0.20
	"estate_power(estate_type:nobles_estate)" > 0.20
	at_war = no
}
```

This encodes secularization: no military-order reform, weak clergy, strong nobles with land rights.

# Designing estate-aware reform paths

1. **Privilege → reform lock** — Keep flavor reforms tied to privileges players must revoke to transform (Order → secular state).
2. **Reform → estate modifiers** — Use satisfaction and power modifiers instead of scripting monthly estate events when possible.
3. **Formable allow** — Combine `has_reform`, `has_estate_privilege`, and `estate_power()` for transformation milestones.
4. **Tooltips** — Explain estate requirements in loc; use `custom_tooltip` in `allow` when logic spans scripted triggers.

# blocked_from_forming_countries

Some reforms block all formables via modifier:

```txt
country_modifier = {
	blocked_from_forming_countries = yes   # papacy_reform
}
```

Check this when a country "cannot form anything" during debugging — the active reform may forbid formation.

# See also

* [Government reforms](government-reforms.md)
* [Formable triggers and effects](/formables/formable-triggers-and-effects.md)

# Citations

[1] Vanilla: `game/in_game/common/government_reforms/theocracy.txt` — `military_order_reform` with `locked = { has_estate_privilege = estate_privilege:clergy_military_orders }` and estate modifiers
[2] Vanilla: `game/in_game/common/formable_countries/00_formable_countries.txt` — `PRU_f` `allow` estate power / privilege gates
[3] Vanilla: `game/in_game/common/government_reforms/country_specific.txt` — `papacy_reform` with `blocked_from_forming_countries = yes`
[4] Vanilla: `game/main_menu/common/static_modifiers/country.txt` — `blocked_from_forming_countries` modifier type definition
[5] Mod: `mod/northern_crusade_teu/in_game/common/government_reforms/teu_nc_reforms.txt` — mod estate-aware reforms (e.g. `teu_nc_officer_aristocracy`)
