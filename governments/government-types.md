---
type: Reference
title: Government types
description: Defining custom government_type entries — succession, power meters, and when to add a new type vs a reform only.
resource: game/in_game/common/government_types/00_default.txt
tags: [governments, government-types, succession, syntax]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

Government types are defined in `in_game/common/government_types/`. Vanilla ships five in `00_default.txt`: `monarchy`, `republic`, `theocracy`, `steppe_horde`, `tribe`.

# Anatomy of a type

```txt
monarchy = {
	use_regnal_number = yes
	heir_selection = cognatic_primogeniture
	heir_selection = salic_law
	# … more heir_selection options …
	map_color = gov_monarchy
	government_power = legitimacy
	default_character_estate = nobles_estate
	modifier = {
		care_about_producing_heirs = yes
	}
}
```

| Field | Purpose |
|-------|---------|
| `heir_selection` | One or more allowed succession laws for this type |
| `use_regnal_number` | Regnal numbering for rulers |
| `generate_consorts` | Whether consorts are generated (hordes, tribes) |
| `map_color` | Map/graphical reference |
| `government_power` | Primary ruler power resource (`legitimacy`, `devotion`, `republican_tradition`, …) |
| `default_character_estate` | Default estate for characters (`nobles_estate`, `clergy_estate`, `burghers_estate`) |
| `modifier` | Baseline modifiers for all countries of this type |

# When to add a custom type

Add a new type when formation or flavor needs a **different succession set or power meter** than any vanilla type — not just extra modifiers (those belong on reforms).

`northern_crusade_teu` defines five custom types in `government_types/teu_nc_governments.txt`:

| Type | Based on | Used when forming |
|------|----------|-------------------|
| `teu_nc_military_state` | Monarchy-like | Orderstaat (`ODR_f`) |
| `teu_nc_sacral_monarchy` | Theocracy-like | Holy Prussia (`HPR_f`) |
| `teu_nc_merchant_protectorate` | Republic-like | Prussian League (`PRL_f`) |
| `teu_nc_imperial_military_monarchy` | Monarchy | Empire of the North (`ENR_f`) |
| `teu_nc_maritime_federation` | Republic | Baltic League (`BLC_f`) |

Each type reuses familiar patterns (e.g. `government_power = legitimacy` on military monarchy branches) so UI and balance stay readable.

# Changing type in content

```txt
change_government_type = government_type:monarchy
change_government_type = government_type:teu_nc_sacral_monarchy
```

Vanilla `PRU_f` `form_effect` switches non-monarchy/non-republic countries to `monarchy` when forming Prussia. Pirate events switch to `republic`.

Always pair type changes with appropriate reforms if the new type expects a signature major reform.

# Reforms referencing custom types

Reforms use the type key in `government =`:

```txt
teu_nc_military_state_reform = {
	government = teu_nc_military_state
	major = yes
	# …
}
```

If `government =` does not match the country's current type, the reform cannot be added (UI: `ADD_REFORM_WRONG_TYPE`).

# Localization

Add display name and description keys matching the type ID:

```yml
 teu_nc_military_state: "Military State"
 teu_nc_military_state_desc: "A state where discipline is citizenship and the sword is law."
```

Mod example: `northern_crusade_teu/main_menu/localization/english/teu_nc_l_english.yml` (reforms and types in one flavor file is fine for small mods).

# See also

* [Reforms and government types](reforms-and-types.md)
* [Government reforms](government-reforms.md)
* [Formable triggers and effects](/formables/formable-triggers-and-effects.md)

# Citations

[1] Vanilla: `game/in_game/common/government_types/00_default.txt` — `monarchy`, `republic`, `theocracy`, `steppe_horde`, `tribe`
[2] Mod: `northern_crusade_teu/in_game/common/government_types/teu_nc_governments.txt` — five custom formation types
[3] Vanilla: `game/in_game/common/government_reforms/readme.txt` — reform field schema; `government =` type compatibility
