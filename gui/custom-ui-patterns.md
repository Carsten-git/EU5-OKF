---
type: Playbook
title: Custom UI patterns
description: Scripted GUI controllers, variable ring-buffer charts, aaa_ template precedence, and hidden building lists.
tags: [gui, scripted-gui, charts, templates]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

EU5 has no first-class chart widget and limited UI extension APIs. Large mods still ship custom economy panels by combining **scripted GUI**, **country variables**, and careful **template override order**.

# Scripted GUI as controller

Keep business logic in scripted effects / variables. GUI only binds:

```txt
# Conceptual GUI call
GetScriptedGui('mnt_income_graph_metric_toggle').Execute(...)
GetScriptedGui('mnt_is_building_shown').IsShown(...)
```

Use scripted GUIs for toggles (RGO visibility, graph mode/frequency) and visibility gates.

# Ring-buffer charts

1. Monthly hidden event pushes `monthly_balance` (etc.) into `hist_01` ← shift from `hist_02`…`hist_50`.
2. Normalize into bar heights for progressbar widgets.
3. Tooltips read date + value vars.

Budget ~50 slots before the scripted-effect file becomes unmaintainable. Prefer player-only updates for heavy series.

# `aaa_` template precedence

Named GUI templates are **first-definition-wins**. Prefix override files so they sort first:

```
in_game/gui/shared/aaa_epbm_expense_tooltip.gui
```

Comment that the file is a full template copy and must be re-synced on vanilla UI patches.

# Global hidden building list

For large classification sets (all RGO buildings):

1. At game start, fill `global_variable_list` with building types.
2. Scripted GUI checks membership + player toggle var.
3. Version the init flag when the list membership changes (`…_initialized_v2`).

# Analyzer silencer

Variables only referenced from GUI loc can trip “never set / never used” warnings. MnT uses a hidden orphan event with an impossible trigger that touches those vars (`mnt_never_trigger_me` pattern) — a known engine QA workaround.

# Loc placement

Default interface strings to `main_menu/localization/`. Keep only session-critical GUI keys in `in_game/localization/` if needed. See [Localization](/localization/).

# Citations

[1] MnT `in_game/common/scripted_guis/`
[2] MnT `in_game/common/scripted_effects/MnT_income_graph.txt`
[3] MnT `in_game/gui/shared/aaa_epbm_expense_tooltip.gui`
[4] MnT `in_game/events/MnT_never_trigger_me.txt`
[5] [Three-root architecture](/total-conversion/three-root-and-override-ladder.md)
