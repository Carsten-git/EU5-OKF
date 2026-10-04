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

# Invalid tooltip when greyed (FB-014 pattern)

ScriptedGui has no `invalid_tooltip` field. For EU5-style **conditions** list on a greyed button:

1. **`tooltipwidget`** on the button (shows while disabled — unlike `action_tooltip` alone).
2. **`action_tooltip` → `conditions`** — loc key with per-line `@trigger_yes!` / `@trigger_no!` text.
3. **`type = location` customizable localization** — one entry per gate; pick yes/no loc key from trigger.
4. Composite loc key joins lines via `[Location.Custom('gate_line')]`.

```gui
tooltipwidget = { using = my_button_tooltip }

action_tooltip = {
	conditions = "my_open_conditions_tt"
	enabled = "[GetScriptedGui('my_open').IsValid(...)]"
	…
}
```

```yml
my_open_conditions_tt: "[Location.Custom('my_tt_line_a')]\n[Location.Custom('my_tt_line_b')]"
my_tt_line_a_yes: "@trigger_yes! Requirement met"
my_tt_line_a_no: "@trigger_no! Requirement failed"
```

Inside `my_button_tooltip`, define the template in **`gui/aaa_*.gui`** (or another file that sorts **before** the host widget file). If `location_window.gui` references `using = my_button_tooltip` before the template file loads, the engine drops the button with no obvious in-game error.

Use **`TooltipTextBlock`** for invalid-state lines. Build multi-line **`text`** via **binary-only** **`Concatenate`** in GUI scope — EU5 rejects 3+ arguments. Join lines with **`'\\n'`** in `.gui` sources (single `\` breaks the lexer). Prefer **`tooltipwidget`** for greyed gate text; omit heavy `Concatenate` from `action_tooltip` **`conditions`** unless validated in-game.

```gui
TooltipTextBlock = {
	visible = "[Not(GetScriptedGui('my_open').IsValid(...))]"
	blockoverride "text" {
		text = "[Concatenate(Location.Custom('my_tt_line_a'), Concatenate('\\n', Location.Custom('my_tt_line_b')))]"
	}
}
```

Do **not** put `Location.Custom()` inside a static loc key passed to `textcontext` — it will not expand and shows "Conditions: none".

# See also

* [UI override file layering](/gui/ui-override-file-layering.md) — KI-063 IsShown trap
* Vanilla `scripted_guis.info` — `is_shown` vs `is_valid`

# Citations

[1] Vanilla `in_game/common/scripted_guis/scripted_guis.info`
[2] RGO Conversion Convert button — IsValid greying on cooldown / busy / peasants
