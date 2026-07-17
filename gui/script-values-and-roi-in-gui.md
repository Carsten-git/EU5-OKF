---
type: Playbook
title: Script values and ROI in GUI
description: Expose computed costs via script_values in GuiScope; drive ROI display modes with CMM dropdown aliases.
tags: [gui, script-values, economy, cmm, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp patterns **#12 / #13**.

# Script values for GUI (#12)

Define scoped script values (e.g. `glorpui_rgo_expand_cost` expecting `scope:cm_location` / `scope:cm_country`). Call from GUI:

```txt
GuiScope.AddScope(...).ScriptValue('glorpui_rgo_expand_cost')
```

Use when displayed cost differs from vanilla API or needs peer-mod context (Construction Manager scopes).

# Dropdown aliases → ROI mode (#13)

CMM dropdown options sync to aliases (`roiUseProfit`, `roiUseMonths`). Panels:

```txt
visible = "[Player.MakeScope.GetVariable('roiUseMonths').IsSet]"
# Select_CFixedPoint(...) for profit vs months formulas
```

Share tooltip templates in a types file (`glorpUI_roi_tooltip_base`).

# Related

* [CMM aliases for GUI](/community-mod-framework/cmm-aliases-for-gui.md)
* [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md)

# Citations

[1] Glorp `in_game/common/script_values/glorpui_rgo_script_values.txt`
[2] Glorp `in_game/gui/expand_raw_goods_lateralview.gui`, `glorpUI_shared_types.gui`
[3] Glorp `glorpUI_build_location_lateralview.gui`, `production_lateralview.gui`
