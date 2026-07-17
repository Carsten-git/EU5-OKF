---
type: Reference
title: Advance and mission localization
description: Loc key patterns for advances and mission trees — names, descriptions, and mission UI suffixes.
resource: mod/northern_crusade_teu/main_menu/localization/english/advances/
tags: [localization, advances, missions]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# Advances

Advance DB keys map directly to localization entries in `main_menu/localization/english/` (often `advances_l_english.yml` or a mod subfolder).

| Key | Purpose |
|-----|---------|
| `feudalism_advance` | Display name |
| `feudalism_advance_desc` | Flavor paragraph |

Mod example (`northern_crusade_teu/main_menu/localization/english/advances/teu_nc_advances_l_english.yml`):

```yaml
 teu_nc_last_crusade: "Last Crusade"
 teu_nc_last_crusade_desc: "The Baltic frontier remains a field of holy war. Unlocking this advance grants #G +5 Purpose#! once."
```

Rules:

- Key must match the advance id in `in_game/common/advances/<file>.txt`.
- `_desc` suffix is mandatory for tooltip body text.
- Use game concept links (`[advance|e]`) in longer descriptions when matching vanilla tone.

See [Starting technology level](/advances/starting-technology-level.md) for gameplay-side advance fields.

# Missions

Mission trees use the **mission id** and **task id** as loc roots. Vanilla generic pack (`generic_missions_l_english.yml`):

| Key pattern | UI slot |
|-------------|---------|
| `generic_conquer_province` | Tree title |
| `generic_conquer_province_DESCRIPTION` | Short subtitle |
| `generic_conquer_province_CRITERIA_DESCRIPTION` | Completion criteria |
| `generic_conquer_province_BUTTON_TOOLTIP` | Selection tooltip |
| `generic_conquer_province_BUTTON_DETAILS` | Detail panel |
| `mission_war_chest` | Task name |
| `mission_war_chest_desc` | Task description |
| `mission_war_chest_tip` | Optional hint (links, arrows) |

Mod tree `teu_nc_reform_order` / tasks `teu_nc_odr_a1_military_state` in `teu_nc_missions_l_english.yml`:

```yaml
 teu_nc_reform_order: "Reform the Order"
 teu_nc_odr_a1_military_state: "Military State"
```

Add `_desc` / `_tip` suffixed keys when tasks need more than a title line.

# File placement

| Content | Suggested path |
|---------|----------------|
| Mod advances | `main_menu/localization/english/advances/<mod>_l_english.yml` |
| Mod missions | `main_menu/localization/english/missions/<mod>_l_english.yml` |
| Mission events | `…/events/…` per [event localization naming](event-localization-naming.md) |

Keep UTF-8 BOM — see [UTF-8 BOM requirement](utf8-bom-requirement.md).

# Cross-references in text

Mission tips often use `Link('hints', …)` and icon concepts copied from vanilla generic missions. Advance desc can reference [custom loc](/customizable-localization/country-scoped-custom-loc.md) indirectly via event rewards ("+#G Purpose#!").

# See also

* [Localization key conventions](localization-key-conventions.md)
* [Mission trees overview](/missions/mission-trees-overview.md)
* [Dynamic text in loc](dynamic-text-in-loc.md)

# Citations

[1] Vanilla: `main_menu/localization/english/advances_l_english.yml`, `missions/generic_missions_l_english.yml`
[2] Mod: `northern_crusade_teu/main_menu/localization/english/advances/teu_nc_advances_l_english.yml`
