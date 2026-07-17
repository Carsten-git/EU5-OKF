---
type: Reference
title: Mission localization
description: Loc keys for missions and tasks under main_menu/localization/english/missions/.
resource: mod/northern_crusade_teu/main_menu/localization/english/missions/
tags: [missions, localization]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

Mission UI text is **not** inline in mission `.txt` files. Keys match mission and task IDs defined in `in_game/common/missions/`, with suffix conventions for extended copy.

# File location

```
main_menu/localization/english/missions/
├── generic_missions_l_english.yml          # generic pack missions + tasks
├── generic_mission_events_l_english.yml    # shared mission event strings
├── generic_conquest_mission_events_l_english.yml
└── <mod>_missions_l_english.yml          # mod-added trees
```

Mod mirror path:

```
<mod>/main_menu/localization/english/missions/teu_nc_missions_l_english.yml
```

All `.yml` files need **UTF-8 BOM** — see [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md).

# Mission (chain) keys

For mission ID `generic_trade`:

| Key | UI slot |
|-----|---------|
| `generic_trade` | Short title on mission card |
| `generic_trade_DESCRIPTION` | Subtitle / flavor line |
| `generic_trade_CRITERIA_DESCRIPTION` | Completion criteria summary |
| `generic_trade_BUTTON_TOOLTIP` | Hover on mission button |
| `generic_trade_BUTTON_DETAILS` | Expanded description in picker |

Example from vanilla:

```yaml
generic_trade: "The Development of Trade"
generic_trade_DESCRIPTION: "If we are to further our prosperity, we must look to promote our trade endeavors!"
generic_trade_CRITERIA_DESCRIPTION: "We have shaped our merchant policy and improved the relevant infrastructure."
generic_trade_BUTTON_TOOLTIP: "The competitive world of trade offers lucrative opportunities..."
generic_trade_BUTTON_DETAILS: "The world of business is full of cutthroat merchants..."
```

Country-specific mod missions need the same five keys per mission block ID (`teu_nc_mission_last_crusade`, `teu_nc_reform_order`, …).

# Task keys

For task ID `mission_conquer_province`:

| Key | UI slot |
|-----|---------|
| `mission_conquer_province` | Task title in tree |
| `mission_conquer_province_desc` | Task description body |
| `mission_conquer_province_tip` | Optional player hint (links, `@arrow_bonus_tier_*` icons) |

Not every vanilla task defines `_tip`; add it when the task needs panel navigation hints.

# Tooltip keys referenced from script

Triggers and effects reference loc keys by string — these are **separate** entries:

```txt
custom_tooltip = {
	text = requirements_after_mission_conquer_province_tt
	always = no
}
```

```yaml
requirements_after_mission_conquer_province_tt: "The requirements of this task will be unveiled after we complete the $mission_conquer_province$ [mission_task|e]"
```

Other common patterns:

* `*_tt` — requirement or reward tooltips shown in condition tooltips
* `$other_key$` — embed another loc key inline
* `[concept|e]` — game encyclopedia links (e.g. `[mission|e]`, `[casus_belli|e]`)
* `[Link('panel', 'tab', '@icon! Label')]` — in-game deep links (vanilla `_tip` fields)

# select_trigger and shared strings

Picker dialogs reuse global keys in the same folder:

```yaml
select_mission_for_next_mission_tasks: "Select a [province|e] for the next [mission task|e]"
mission_task_select_market: "Select a [market|e] for the [task|e]"
integrate_province_no_provinces: "..."   # matches none_available_msg_key
```

The `name` and `none_available_msg_key` in `select_trigger` must exist in localization.

# Mission event localization

Events fired from `on_monthly` use their own namespace files:

* `generic_conquest_mission_events_l_english.yml` — titles/descriptions for `conquest_mission_events.*`

Follow [Event localization naming](/localization/event-localization-naming.md) for option keys.

# Mod generation pattern

Northern Crusade generates mission loc from mission IDs:

```python
# tools/generate_localization.py — generate_missions_loc()
for pack, name in PACK_NAMES.items():
    lines.append(f" {pack}: {yaml_str(name)}")
for tid in collect_mission_task_ids():
    lines.append(f" {tid}: {yaml_str(mission_title(tid))}")
```

Generated output is minimal (title only per task). Add `_desc`, `_tip`, and `_*_DESCRIPTION` keys manually or extend the generator for full vanilla parity.

Hand-authored tooltip example in the same generator:

```yaml
teu_nc_teu_a5_face_grunwald_tt: "Complete the Tannenberg event chain without suffering defeat."
```

# Naming checklist

1. Mission block key in `.txt` **equals** base loc key.
2. Task key in `.txt` **equals** base loc key.
3. Add `_desc` for every player-facing task; add `_tip` when UI guidance helps.
4. Add five mission-level keys for picker cards on non-generic trees.
5. Every `custom_tooltip = { text = foo_tt }` needs a `foo_tt` entry.
6. Static modifiers referenced via `[ShowModifier('id')]` need [static modifier loc](/localization/static-modifier-localization.md).

# Examples

| Content | File |
|---------|------|
| Full generic mission + task loc | `game/main_menu/localization/english/missions/generic_missions_l_english.yml` |
| Conquest mission events | `generic_conquest_mission_events_l_english.yml` |
| Mod mission titles | `northern_crusade_teu/main_menu/localization/english/missions/teu_nc_missions_l_english.yml` |

# Citations

Vanilla mission card keys:

```14:21:c:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\main_menu\localization\english\missions\generic_missions_l_english.yml
generic_conquer_province: "Regional Expansion"
generic_conquer_province_DESCRIPTION: "Onwards to Glory!"
generic_conquer_province_CRITERIA_DESCRIPTION: "Our [ROOT.GetCountry.GetFlavorRank] has expanded its territories."
generic_conquer_province_BUTTON_TOOLTIP: "Proactive offensives are necessary..."
generic_conquer_province_BUTTON_DETAILS: "The borders that surround our domain..."
choose_province_for_next_conquest_tasks_tt: "We will choose the designated [province|e] for the following [mission_tasks|e]"
```

# See also

* [Mission trees overview](mission-trees-overview.md)
* [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)
* [Dynamic text in loc](/localization/dynamic-text-in-loc.md)
* [Event localization naming](/localization/event-localization-naming.md)
* [Static modifier localization](/localization/static-modifier-localization.md)
