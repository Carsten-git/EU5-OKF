---
type: Reference
title: Vanilla file locations
description: Where to read working EU5 examples inside the Steam game install.
resource: game/
tags: [references, vanilla, paths]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

# Default install (Windows)

```
C:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\
├── in_game\
│   ├── common\
│   │   ├── advances\              # country_TEU.txt, 0_age_of_traditions.txt, …
│   │   ├── on_action\             # _hardcoded.txt, country pulses
│   │   ├── scripted_effects\
│   │   ├── scripted_triggers\
│   │   ├── missions\              # generic_*_mission_pack.txt, ____Info.txt
│   │   ├── government_reforms\
│   │   ├── government_types\
│   │   ├── formable_countries\
│   │   ├── country_interactions\
│   │   ├── customizable_localization\
│   │   └── tests\                 # scenario tests (advanced)
│   ├── setup\
│   │   └── countries\             # tag metadata (baltics.txt, …)
│   └── events\
│       ├── DHE\                   # flavor_HUN.txt, flavor_teu.txt, …
│       ├── situations\
│       └── government\
└── main_menu\
    ├── common\
    │   ├── static_modifiers\      # country.txt, location.txt
    │   └── modifier_type_definitions\
    │       └── 00_modifier_types.txt   # authoritative modifier stat keys
    └── localization\
        └── english\
            ├── advances_l_english.yml
            ├── events\DHE\
            ├── static_modifiers_l_english.yml
            ├── government_reforms_l_english.yml
            └── missions\
```

# User directories (Windows)

```
Documents/Paradox Interactive/Europa Universalis V/
├── mod/
│   ├── eu5-modding-knowledge/    # this bundle
│   └── northern_crusade_teu/     # example flavor mod
└── logs/
    ├── error.log                 # primary mod debugging
    ├── game.log
    └── debug.log
```

# Good vanilla references

| Topic | Path |
|-------|------|
| DHE events | `in_game/events/DHE/flavor_HUN.txt` |
| TEU flavor events | `in_game/events/DHE/flavor_teu.txt` |
| Scripted triggers | `in_game/common/scripted_triggers/country_triggers.txt` |
| Customizable loc | `in_game/common/customizable_localization/country_history.txt` |
| Country tag metadata | `in_game/setup/countries/baltics.txt` |
| Bookmark start state | `main_menu/setup/start/10_countries.txt` |
| TEU country advances | `in_game/common/advances/country_TEU.txt` |
| Global advance tree | `in_game/common/advances/0_age_of_traditions.txt` |
| `on_game_start` | `in_game/common/on_action/_hardcoded.txt` |
| Chained on_actions | `in_game/common/on_action/ai_personalities_setup.txt` |
| Event triggering | `in_game/events/situations/western_schism.txt` |
| Generic missions | `in_game/common/missions/generic_conquer_province_mission_pack.txt` |
| Mission field schema | `in_game/common/missions/____Info.txt` |
| Government types | `in_game/common/government_types/00_default.txt` |
| Government reforms | `in_game/common/government_reforms/readme.txt` |
| Formable countries | `in_game/common/formable_countries/` |
| Modifier stat keys | `main_menu/common/modifier_type_definitions/00_modifier_types.txt` |
| Static modifier examples | `main_menu/common/static_modifiers/country.txt` |
| Static modifier loc | `main_menu/localization/english/static_modifiers_l_english.yml` |
| Advance loc | `main_menu/localization/english/advances_l_english.yml` |

# Mod reference (Northern Crusade)

| Topic | Path under `northern_crusade_teu/` |
|-------|-------------------------------------|
| Mod metadata | `.metadata/metadata.json` |
| Country advances | `in_game/common/advances/count_TEU_northern_crusade.txt` |
| Purpose on_actions | `in_game/common/on_action/teu_nc_purpose.txt` |
| Scripted triggers | `in_game/common/scripted_triggers/teu_nc_triggers.txt` |
| Custom loc | `in_game/common/customizable_localization/teu_nc_purpose.txt` |
| Static modifiers | `main_menu/common/static_modifiers/teu_nc_modifiers.txt` |
| Validation | `tools/validate_mod.py` |
| DHE events | `in_game/events/DHE/flavor_teu_nc_*.txt` |

# See also

* [Reading vanilla examples](/getting-started/reading-vanilla-examples.md)
* [Mod folder structure](/getting-started/mod-folder-structure.md)
* [Error log debugging](/validation/error-log-debugging.md)
* [Paradox wiki and tools](paradox-wiki-and-tools.md)

# Citations

[1] Vanilla: `C:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\` — default Windows install
[2] User: `Documents/Paradox Interactive/Europa Universalis V/mod/` — mod and bundle paths
[3] User: `Documents/Paradox Interactive/Europa Universalis V/logs/error.log` — primary mod debugging log
