---
type: Reference
title: Reforms and government types
description: How EU5 splits government types (succession, power meter) from government reforms (policy slots with modifiers).
resource: game/in_game/common/government_reforms/readme.txt
tags: [governments, reforms, government-types, overview]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

EU5 government modding uses **two separate definition layers**:

| Layer | Folder | What it defines |
|-------|--------|-----------------|
| **Government type** | `in_game/common/government_types/` | Base category: succession laws, `government_power` meter (legitimacy, devotion, …), default estate, map color, type-level modifiers |
| **Government reform** | `in_game/common/government_reforms/` | Selectable policies slotted into a country; age gates, `potential`/`allow`, modifiers, activation effects |

A country has exactly one **government type** at a time and zero or more **reforms** in reform slots. Reforms reference a compatible type via `government = monarchy` (or a custom type key).

# Vanilla layout

```
in_game/common/
├── government_types/
│   └── 00_default.txt          # monarchy, republic, theocracy, steppe_horde, tribe
└── government_reforms/
    ├── common.txt
    ├── monarchy.txt
    ├── republic.txt
    ├── theocracy.txt
    ├── steppe_horde.txt
    └── country_specific.txt    # papacy, military orders, nation-unique reforms
```

Each reform file's header comment (`readme.txt`) documents supported fields.

# How they interact in play

- **Type** sets the frame: which heir-selection laws appear, which power bar the UI shows, default character estate.
- **Reforms** stack modifiers and can unlock mechanics (`allow_military_order_units`, `blocked_from_forming_countries`, estate levy rules).
- **`change_government_type`** switches the type (often in events or `form_effect`).
- **`add_reform = government_reform:my_reform`** adds a reform without necessarily changing type.
- **`has_reform = government_reform:my_reform`** is the standard trigger for events, formables, and reform `potential` blocks.

Major reforms (`major = yes`) are mutually exclusive — only one major reform per country.

# Mod pattern: custom type + matching reform

`northern_crusade_teu` adds both layers for the Orderstaat path:

1. Custom type `teu_nc_military_state` in `government_types/teu_nc_governments.txt` (succession, `government_power = legitimacy`).
2. Reform `teu_nc_military_state_reform` with `government = teu_nc_military_state` so it only appears on that type.
3. Formable `ODR_f` calls `change_government_type` and `add_reform` together in `form_effect`.

# Localization split

| Content | Loc file (vanilla) | Key pattern |
|---------|-------------------|-------------|
| Reforms | `main_menu/localization/english/government_reforms_l_english.yml` | `reform_key`, `reform_key_desc` |
| Types | Often same mod loc file or `government_names_l_english.yml` | `type_key`, `type_key_desc` |

# See also

* [Government reforms](government-reforms.md) — defining reforms, `has_reform`, `add_reform`
* [Government types](government-types.md) — custom types and inheritance
* [Estates and reforms](estates-and-reforms.md) — estate modifiers inside reforms
* [Formable triggers and effects](/formables/formable-triggers-and-effects.md) — government changes on formation

# Citations

[1] Vanilla: `game/in_game/common/government_types/00_default.txt`
[2] Vanilla: `game/in_game/common/government_reforms/readme.txt`, `monarchy.txt`, `country_specific.txt`
[3] Mod: `northern_crusade_teu/in_game/common/government_types/teu_nc_governments.txt`, `government_reforms/teu_nc_reforms.txt`
