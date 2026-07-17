---
type: Playbook
title: CMM pause menu entry
description: Open Community Mod Menu from the ESC pause menu — host vs non-host CMF scripted GUIs (Glorp pattern #29).
tags: [cmf, cmm, gui, pause-menu, multiplayer, glorp]
timestamp: 2026-07-12T22:45:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#29**: inject a **Mod Settings** button into the pause (`ingame_menu`) template that opens CMM when CMF is active, with **different CMF scripted GUIs for host vs non-host** in multiplayer.

# Entry points in Glorp

File: `in_game/gui/glorpUI_ingame_menu.gui` (overrides vanilla pause menu middle section).

| Button | Visible when | Opens CMM via |
|--------|--------------|---------------|
| CMM (host / SP) | `cmf_active` AND (`Not(GameIsMultiplayer)` OR `IsHost`) | `CMM_SetHostAndRegisterCoreMod` |
| CMM (MP client) | `cmf_active` AND `GameIsMultiplayer` AND `Not(IsHost)` | `CMM_RegisterCoreMod` |
| Glorp fallback | `Not(cmf_active)` | `gui.createwidget` → `glorpUI_window.gui` |

# Host / singleplayer button

```txt
button_wax = {
	text = "CMM_PAUSE_MENU_BUTTON"
	visible = "[And(GetVariableSystem.Exists('cmf_active'), Or(Not(GameIsMultiplayer), IsHost))]"
	onclick = "[GetScriptedGui('CMM_SetHostAndRegisterCoreMod').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).End)]"
	onclick = "[GetVariableSystem.Clear('cmm_mod_search_text')]"
	onclick = "[GetVariableSystem.Toggle('cmm_window_open')]"
	onclick = "[PauseMenu.Resume]"
}
```

`CMM_SetHostAndRegisterCoreMod` — host can edit global CMM settings and registers core mod state for the session.

# Multiplayer non-host button

```txt
visible = "[And(GetVariableSystem.Exists('cmf_active'), And(GameIsMultiplayer, Not(IsHost)))]"
	onclick = "[GetScriptedGui('CMM_RegisterCoreMod').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).End)]"
```

Non-hosts use `CMM_RegisterCoreMod` only — cannot change global settings.

# Why toggle `cmm_window_open` after Execute

Sequence on click:

1. Register / refresh CMM host state (`Execute`).
2. Clear mod search filter.
3. Toggle CMM window open.
4. `PauseMenu.Resume` — closes pause overlay so CMM draws over game (Glorp choice).

Other mods may skip `Resume` if they prefer CMM over the dimmed pause backdrop — test visually.

# Detecting CMF at GUI time

```txt
GetVariableSystem.Exists('cmf_active')
```

Set by CMF when loaded. Do not use mod metadata from GUI — only runtime vars.

# Fallback when CMF missing

```txt
visible = "[Not(GetVariableSystem.Exists('cmf_active'))]"
text = "glorpui_name"
	onclick = "[ExecuteConsoleCommand( Select_CString( GetVariableSystem.Exists('glorpUI_window_open'), 'gui.ClearWidgets glorpUI_window', 'gui.createwidget gui/glorpUI_window.gui glorpUI_window' ) )]"
	onclick = "[GetVariableSystem.Toggle('glorpUI_window_open')]"
```

Register widget in `scripted_widgets/glorpUI_scripted_windows.txt`. Full pattern: [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md).

# Applying to your mod

Content mods with CMM settings **do not** need their own pause-menu button — players open **Community Mod Framework** in CMM and pick your mod’s tab.

Add a pause button only if:

* You want a branded shortcut (“RGO Conversion settings”), or
* You ship a non-CMF fallback window.

Otherwise document: **ESC → Community Mod Menu → [your mod tab]**.

# Loc

Use CMF’s shared keys where possible:

* `CMM_PAUSE_MENU_BUTTON`
* `CMM_PAUSE_MENU_BUTTON_TOOLTIP`

Mod name/desc still come from `[mod_id]_name` / `_desc` in your CMM loc file.

# Related

* [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md)
* [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md)
* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)

# Citations

[1] Glorp `in_game/gui/glorpUI_ingame_menu.gui`
[2] Glorp `in_game/gui/scripted_widgets/glorpUI_scripted_windows.txt`
[3] CMF wiki — Community Mod Menu
