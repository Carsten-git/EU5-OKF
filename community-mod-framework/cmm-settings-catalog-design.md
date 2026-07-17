---
type: Playbook
title: CMM settings catalog design
description: Organize many player toggles into CMM tabs, groups, and defaults — Glorp UI worked example (pattern #27).
tags: [cmf, cmm, settings, ux, glorp]
timestamp: 2026-07-12T22:45:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#27**: a UI overhaul with ~15 player-facing options does **not** use a new-game wizard. It ships a **structured CMM catalog** — tabs by UI area, groups within tabs, `default_value` encoding out-of-box UX, aliases for `.gui`, and a unified `cmf_on_callback` switch.

# When to use this pattern

| Use CMM catalog | Use something else |
|-----------------|-------------------|
| Cosmetic / QoL toggles any time in campaign | One-time country setup → events or `country_setup` |
| Per-player UI preferences | Host-only rules for all players → `cmm_register_global_*` |
| Settings players revisit | Rare pre-game choices → vanilla game rules (if modding supported) |

See also [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md).

# Glorp catalog structure (v1.3.10.1)

Registration lives in `glorpui_register_cmf_mod` (`in_game/common/scripted_effects/glorpui_cmm_effects.txt`).  
Localization: `main_menu/localization/english/glorpui_cmm_l_english.yml`.

| Tab `tab_id` | Group `group_id` | Settings | Role |
|--------------|------------------|----------|------|
| `toggles` | `general` | 6 bools | Portraits, flags, left-panel chrome |
| `top_bar` | `general` | 7 bools | Hide/show top-bar stats, dynamic war stats |
| `mapmodes_bar` | `general` | 3 bools | Mapmode button colours and sizing |
| `build_panel` | `general` | 2 dropdowns | ROI metric + ROI time unit |

Loc keys follow CMF convention: `glorpui__{tab_id}_name`, `glorpui__{tab_id}__{group_id}_name`, `glorpui__{setting_id}_name` / `_desc`.

# Registration recipe per setting

Standard bool (feature **on** when toggle enabled):

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

**Inverted bool** (setting label says “disable X”; feature **on** when CMM value is 0):

```txt
cmm_register_bool_setting = {
	mod_id = glorpui
	setting_id = disableLeftPanelStatsBar
	tab_id = toggles
	group_id = general
	default_value = 1
}
cmm_sync_bool_alias_inverted = {
	setting = glorpui__disableLeftPanelStatsBar
	alias = disableLeftPanelStatsBar
}
```

Glorp uses inverted aliases heavily for top-bar and mapmode “hide” toggles so **defaults hide clutter** (`default_value = 1` = disabled/hidden in UI).

Dropdown with option alias:

```txt
cmm_register_dropdown_setting = {
	mod_id = glorpui
	setting_id = roi_metric
	tab_id = build_panel
	group_id = general
	default_index = 1
	option_count = 2
}
cmm_set_dropdown_multiselector = { setting = glorpui__roi_metric }
cmm_sync_dropdown_option_alias = {
	setting = glorpui__roi_metric
	index = 2
	alias = roiUseProfit
}
```

See [CMM dropdown multiselector](/community-mod-framework/cmm-dropdown-multiselector.md).

# Dependent setting visibility

Child setting only relevant when parent is on — register scripted GUI after parent:

```txt
cmm_add_scripted_gui = {
	mod_id = glorpui
	setting_id = useFramedStaticFlag
}

# in_game/common/scripted_guis/glorpui_cmm_scripted_gui.txt
glorpui__useFramedStaticFlag_on_changed = {
	scope = country
	is_shown = { has_variable = useStaticFlag }
	effect = { }
}
```

Parent `useStaticFlag` must sync alias in registration + callback so `has_variable` is live.

# Callback switch (keep aliases in sync)

Glorp mirrors every alias in `glorpui_handle_cmf_callback` with a `switch` on `var:cmf_callback`:

```txt
glorpui_handle_cmf_callback = {
	switch = {
		trigger = var:cmf_callback
		flag:glorpui__useVanillaPortrait = {
			cmm_sync_bool_alias = {
				setting = glorpui__useVanillaPortrait
				alias = useVanillaPortrait
			}
		}
		flag:glorpui__disableLeftPanelStatsBar = {
			cmm_sync_bool_alias_inverted = {
				setting = glorpui__disableLeftPanelStatsBar
				alias = disableLeftPanelStatsBar
			}
		}
		# … every registered setting
	}
}
```

Without callback resync, `.gui` reads stale `has_variable` state after mid-game toggles.

# GUI consumption

After aliases sync, gate widgets with short vars — not CMM map keys:

```txt
visible = "[Player.MakeScope.GetVariable('useVanillaPortrait').IsSet]"
visible = "[Not(Player.MakeScope.GetVariable('disableLeftPanelStatsBar').IsSet)]"
visible = "[Player.MakeScope.GetVariable('roiUseProfit').IsSet]"
```

Full alias patterns: [CMM aliases for GUI](/community-mod-framework/cmm-aliases-for-gui.md).

# Fallback scripted GUIs (no CMM)

`glorpUI_custom_actions.txt` duplicates toggle logic for the local settings window (`SetDisableTopBarIncome`, etc.) and manually updates CMM map when needed:

```txt
cmf_change_variable_map = {
	name = cmm
	key = flag:glorpui__disableColouredMapmodeButtons
	value = 0
}
```

Keeps CMM storage aligned if player later installs CMF. See [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md).

# Applying to a new mod (e.g. RGO conversion)

Example catalog sketch:

| Tab | Settings |
|-----|----------|
| `gameplay` | Enable AI conversions (bool, default on), show cooldown in tooltip (bool) |
| `ui` | Use action bar entry when Glorp detected (bool) |

Register in `cmf_on_mod_registration`; read in triggers with `"variable_map(cmm|flag:rgo_conv__enable_ai)"` or sync to `has_variable` aliases for scripted GUIs.

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [Glorp UI CMF worked example](/community-mod-framework/glorp-ui-cmf-worked-example.md)
* [CMM pause menu entry](/community-mod-framework/cmm-pause-menu-entry.md)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md)

# Citations

[1] Glorp `in_game/common/scripted_effects/glorpui_cmm_effects.txt`
[2] Glorp `main_menu/localization/english/glorpui_cmm_l_english.yml`
[3] Glorp `in_game/common/scripted_guis/glorpui_cmm_scripted_gui.txt`
[4] Glorp `in_game/common/scripted_guis/glorpUI_custom_actions.txt`
