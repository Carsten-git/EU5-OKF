---
type: Playbook
title: ScriptedGui IsValid button greying
description: Keep a custom button visible but grey it out when scripted gates fail — without using IsShown for visibility.
tags: [gui, scripted-gui, ux, location-window]
timestamp: 2026-07-11T21:00:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.1.1"
---

# Problem

A location-panel button that only uses `enabled = "[Location.GetOwner.IsPlayer]"` stays **clickable** while `ScriptedGui.is_valid` is false — click appears to do nothing.

Using `visible = "[GetScriptedGui(…).IsShown(…)]"` is worse when IsShown is false: the widget **disappears** (KI-063).

# Pattern

| Concern | Bind to |
|---------|---------|
| Who sees the button | Simple GUI check, e.g. `Location.GetOwner.IsPlayer` |
| Can they use it | `GetScriptedGui('…').IsValid(GuiScope…)` on **`enabled`** |
| Click | `GetScriptedGui('…').Execute(GuiScope…)` |

```gui
button_regular = {
	visible = "[Location.GetOwner.IsPlayer]"
	enabled = "[And(Location.GetOwner.IsPlayer, GetScriptedGui('my_open').IsValid(GuiScope.SetRoot(GetPlayer.MakeScope).AddScope('target_location', Location.MakeScope).End))]"
	action_tooltip = {
		enabled = "[And(Location.GetOwner.IsPlayer, GetScriptedGui('my_open').IsValid(GuiScope.SetRoot(GetPlayer.MakeScope).AddScope('target_location', Location.MakeScope).End))]"
		on_action = "[GetScriptedGui('my_open').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).AddScope('target_location', Location.MakeScope).End)]"
	}
}
```

```txt
my_open = {
	saved_scopes = { target_location }
	is_shown = { always = yes }   # do not drive widget visible from this
	is_valid = {
		exists = scope:target_location
		scope:target_location = { my_can_open = yes }
	}
	effect = {
		if = {
			limit = { scope:target_location = { my_can_open = yes } }
			# …
		}
	}
}
```

Wrap gate triggers in `custom_description = { text = … }` so failure reasons can surface in tooltips.

# See also

* [UI override file layering](/gui/ui-override-file-layering.md) — KI-063 IsShown trap
* Vanilla `scripted_guis.info` — `is_shown` vs `is_valid`

# Citations

[1] Vanilla `in_game/common/scripted_guis/scripted_guis.info`
[2] RGO Conversion Convert button — IsValid greying on cooldown / busy / peasants
