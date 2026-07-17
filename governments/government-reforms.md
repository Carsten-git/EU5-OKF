---
type: Reference
title: Government reforms
description: Defining government_reform entries, has_reform triggers, and add_reform effects in EU5.
tags: [governments, reforms, has_reform, add_reform, syntax]
timestamp: 2026-07-06T08:40:00+10:00
resource: game/in_game/common/government_reforms/
status: draft
---

Government reforms live in `in_game/common/government_reforms/*.txt`. Each top-level key is a reform ID referenced elsewhere as `government_reform:<key>`.

# Minimal reform

```txt
teu_nc_centralized_command = {
	age = age_2_renaissance
	government = theocracy
	major = yes
	potential = {
		has_or_had_tag = TEU
	}
	country_modifier = {
		global_crown_estate_power = 0.1
		fort_limit_modifier = 0.1
	}
	years = 2
}
```

# Key fields

| Field | Purpose |
|-------|---------|
| `government` | Restricts reform to a government type key (vanilla `monarchy`, mod custom `teu_nc_military_state`, …) |
| `age` | Earliest age the reform can be adopted |
| `major` | If `yes`, exclusive with other major reforms |
| `unique` | Extra UI treatment for country-specific reforms |
| `potential` | Reform visible/selectable (root = country) |
| `allow` | Additional gate before adoption |
| `locked` | Trigger — reform cannot be removed while true |
| `years` / `months` / … | Implementation time; modifiers scale until fully active |
| `country_modifier` | Always-on country modifiers while active |
| `location_modifier` | Location-scoped modifiers (use `potential_trigger` inside) |
| `on_activate` / `on_fully_activated` / `on_deactivate` | Effect blocks |
| `block_for_rebel` | Rebels cannot use this reform |
| `male_regnal_names` / `female_regnal_names` | Regnal name pools (e.g. papacy) |

See vanilla `government_reforms/readme.txt` for the authoritative comment block.

# has_reform trigger

Always scope the reform with the `government_reform:` prefix:

```txt
# In event trigger, formable allow, reform potential, scripted trigger
has_reform = government_reform:military_order_reform
NOT = { has_reform = government_reform:pirate_brethren_reform }
```

**Vanilla examples**

- `PRU_f` (Prussia formable) blocks formation while `military_order_reform` is active — secularization is implied.
- `military_order_reform` uses `locked = { has_estate_privilege = estate_privilege:clergy_military_orders }` so the reform stays while that privilege exists.
- Event chains (e.g. `flavor_HAB.txt`) gate options on combinations like `hab_geheimer_rat` + `privy_council`.

**Mod example** — `teu_nc_can_form_odr` scripted trigger:

```txt
teu_nc_can_form_odr = {
	custom_tooltip = {
		text = teu_nc_can_form_odr_reform_tt
		has_reform = government_reform:teu_nc_centralized_command
	}
}
```

Loc key `teu_nc_can_form_odr_reform_tt` explains the requirement in the formable UI.

# add_reform effect

Adds a reform immediately (subject to game rules). Used in events, scripted effects, formables, and IO effects.

```txt
add_reform = government_reform:pirate_brethren_reform
add_reform = government_reform:teu_nc_military_state_reform
```

Common pairing with type change:

```txt
change_government_type = government_type:teu_nc_military_state
add_reform = government_reform:teu_nc_military_state_reform
```

Vanilla `pirate_events.txt` switches to `republic` then adds `pirate_brethren_reform`. Country setup in `country_effects.txt` assigns starting reforms (e.g. `daimyo`, `japanese_clan`).

# Mutual exclusion pattern

Use `potential` with `NOT = { has_reform = … }` for peer reforms, or rely on `major = yes` for the UI slot limit:

```txt
feudal_nobility = {
	potential = {
		NOT = { has_reform = government_reform:french_feudal_nobility }
		NOT = { has_reform = government_reform:french_appanage_reform }
	}
}
```

# File organization tips

- Put nation-specific reforms in a dedicated file (mod: `teu_nc_reforms.txt`).
- Match `government =` to the type players will have when selecting the reform.
- If a reform unlocks a formable or mission branch, document the `has_reform` requirement in loc tooltips (`custom_tooltip` in `allow` blocks).

# See also

* [Reforms and government types](reforms-and-types.md)
* [Estates and reforms](estates-and-reforms.md)
* [Formable triggers and effects](/formables/formable-triggers-and-effects.md)

# Citations

[1] Vanilla: `game/in_game/common/government_reforms/readme.txt` — authoritative commented field list
[2] Vanilla: `game/in_game/common/government_reforms/theocracy.txt` — `military_order_reform` (`locked`, `potential`, `country_modifier`)
[3] Vanilla: `game/in_game/common/formable_countries/00_formable_countries.txt` — `PRU_f` blocks `has_reform = government_reform:military_order_reform`
[4] Vanilla: `game/in_game/events/pirate_events.txt` — `change_government_type` + `add_reform = government_reform:pirate_brethren_reform`
[5] Vanilla: `game/in_game/events/DHE/flavor_HAB.txt` — event options gated on reform combinations
[6] Mod: `mod/northern_crusade_teu/in_game/common/government_reforms/teu_nc_reforms.txt` — `teu_nc_centralized_command` and related reforms
[7] Mod: `mod/northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt` — `has_reform` in formable gates
