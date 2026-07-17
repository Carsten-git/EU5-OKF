---
type: Playbook
title: CMM aliases for GUI
description: Sync CMM settings to short country variables for .gui visibility; inverted aliases and dependent scripted_gui.
tags: [cmf, cmm, gui, aliases, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#1 / #9**: GUI should not read long `glorpui__settingId` CMM keys. Sync to short country variables, then gate widgets with `GetVariable('…').IsSet`.

# Register + alias (#1)

```txt
cmm_register_bool_setting = {
	mod_id = glorpui
	setting_id = useVanillaPortrait
	tab_id = toggles
	group_id = general
	default_value = 0
}
cmm_sync_bool_alias = {
	setting = glorpui__useVanillaPortrait
	alias = useVanillaPortrait
}
```

**Inverted aliases** for “disable X” settings (default on in CMM, variable set when feature enabled):

```txt
cmm_sync_bool_alias_inverted = {
	setting = glorpui__disableLeftPanelStatsBar
	alias = disableLeftPanelStatsBar
}
```

Dropdown options → presence aliases:

```txt
cmm_sync_dropdown_option_alias = {
	setting = glorpui__roi_metric
	index = 1
	alias = roiUseProfit
}
```

Re-run alias sync in `cmf_on_callback` when `var:cmf_callback` matches the setting flag.

# GUI read

```txt
visible = "[Player.MakeScope.GetVariable('useVanillaPortrait').IsSet]"
visible = "[Not(Player.MakeScope.GetVariable('disableLeftPanelStatsBar').IsSet)]"
```

# Dependent setting visibility (#9)

After registering a child setting:

```txt
cmm_add_scripted_gui = {
	mod_id = glorpui
	setting_id = useFramedStaticFlag
}
```

Scripted GUI `glorpui__useFramedStaticFlag_on_changed` with `is_shown` requiring parent alias (e.g. `has_variable = useStaticFlag`).

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [CMM lists and advanced](/community-mod-framework/cmm-lists-and-advanced.md)
* [Glorp UI reference](/references/glorp-ui-as-reference.md)

# Citations

[1] Glorp `in_game/common/scripted_effects/glorpui_cmm_effects.txt`
[2] Glorp `in_game/common/scripted_guis/glorpui_cmm_scripted_gui.txt`
[3] Glorp `in_game/gui/glorpUI_hud_topbar.gui`
