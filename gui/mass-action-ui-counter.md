---
type: Playbook
title: Mass-action UI counter
description: Drive batch UI actions with a countdown country variable and progressbar/row visibility animation.
tags: [gui, ux, variables, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#15**: UI-only batch action without a full engine batch API.

1. Scripted GUI `EnableMassRecruiting` sets `massRecruiting = 5` (or N).
2. Recruit rows show progress with visibility vs parent index:

```txt
visible = "[GreaterThan_int32(FixedPointToInt(Player.MakeScope.GetVariable('massRecruiting').GetValue), PdxGuiWidget.AccessParent.GetIndexInParent)]"
```

3. Animation `on_start` calls `UpdateMassRecruiting` (decrement).
4. Finish calls `ClearMassRecruiting`.

Copyable for any “do N times with visible progress” chrome.

# Citations

[1] Glorp `in_game/common/scripted_guis/glorpUI_custom_actions.txt`
[2] Glorp `in_game/gui/recruit_location_lateralview.gui`
