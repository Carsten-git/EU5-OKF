---
type: Playbook
title: Dual settings UI fallback
description: Prefer CMM when CMF is active; otherwise spawn a local settings window; keep CMM map in sync if toggled outside the menu.
tags: [cmf, cmm, gui, fallback, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp patterns **#6 / #10 / #14**: graceful degradation when CMF is missing, plus sync when settings change outside CMM.

# Dual path (#6)

Pause/settings entry — full pause-menu wiring: [CMM pause menu entry](/community-mod-framework/cmm-pause-menu-entry.md) (#29).

1. If `GetVariableSystem.Exists('cmf_active')` → open CMM (`CMM_SetHostAndRegisterCoreMod` / `CMM_RegisterCoreMod` + toggle `cmm_window_open`).
2. Else → spawn fallback window via console createwidget (#14).

# Console createwidget (#14)

```txt
# Conceptual GUI onclick
ExecuteConsoleCommand(
  Select_CString(
    GetVariableSystem.Exists('glorpUI_window_open'),
    'gui.ClearWidgets glorpUI_window',
    'gui.createwidget gui/glorpUI_window.gui glorpUI_window'
  )
)
```

Register path in `scripted_widgets/` so the engine knows the widget. Toggle a variable for open state. Loc stubs for missing CMM keys live in `*_cmm_warning_suppression_l_*.yml` to silence undefined-key noise.

# Sync CMM from outside (#10)

When a fallback scripted GUI toggles a setting, also update CMM storage:

```txt
cmf_change_variable_map = {
	name = cmm
	key = flag:glorpui__disableColouredMapmodeButtons
	value = 1
}
```

(Note: CMF marks some `cmf_change_variable_map` helpers deprecated for general maps; for CMM sync Glorp still uses this pattern — verify against current CMF version.)

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [Dependency check popup](/community-mod-framework/dependency-check-popup.md)

# Citations

[1] Glorp `in_game/gui/glorpUI_ingame_menu.gui`
[2] Glorp `in_game/gui/glorpUI_window.gui`
[3] Glorp `in_game/gui/scripted_widgets/glorpUI_scripted_windows.txt`
[4] Glorp `in_game/common/scripted_guis/glorpUI_custom_actions.txt`
