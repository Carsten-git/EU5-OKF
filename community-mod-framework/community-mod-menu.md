---
type: Playbook
title: Community Mod Menu
description: Register and read CMM toggles, dropdowns, buttons, numeric, and slider settings; react via cmf_on_callback.
tags: [cmf, cmm, settings, gui]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

**CMM** is a shared in-game settings menu. Multiple mods register into one UI. Fastest path: [CMM Visual Editor](/community-mod-framework/toolkit-and-visual-editor.md). Manual path below.

# Register (from your `cmf_on_mod_registration` effect)

Common fields: `mod_id`, `setting_id`, `tab_id`, `group_id`.

```txt
cmm_register_bool_setting = {
	mod_id = your_mod
	setting_id = your_toggle
	tab_id = general
	group_id = general_toggles
	default_value = 0   # 0 off, 1 on
}

cmm_register_dropdown_setting = {
	mod_id = your_mod
	setting_id = your_dropdown
	tab_id = general
	group_id = general_values
	default_index = 1
	option_count = 3
}

cmm_register_button_setting = { … }   # no stored value — use callback
cmm_register_numeric_setting = { … default_value min_value max_value step_value }
cmm_register_slider_setting = { … }   # same numeric knobs
```

Global variants: `cmm_register_global_*` — stored globally; in MP only host edits.

# Read values

Stored in variable map `cmm`, key `flag:[mod_id]__[setting_id]`:

```txt
if = {
	limit = { "variable_map(cmm|flag:your_mod__your_toggle)" >= 1 }
	# enabled
}
```

Globals: `"global_variable_map(cmm|flag:…)"`. If the key comes from a macro, put it in a local var first (macros don’t expand inside quotes).

# React to changes

```txt
cmf_on_callback = {
	on_actions = { your_mod_on_callback }
}

your_mod_on_callback = {
	effect = {
		if = {
			limit = { var:cmf_callback = flag:your_mod__your_toggle }
			# …
		}
	}
}
```

Same callback bus is used for action bar clicks and alert clicks.

# Localization patterns

`[mod_id]_name` / `_desc`, `[mod_id]__[tab_id]_name`, `[mod_id]__[setting_id]_name` / `_desc`, dropdown `[mod_id]__[setting_id]_option_[N]_name`.

# Related

* [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md)
* [CMM lists and advanced](/community-mod-framework/cmm-lists-and-advanced.md)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md)
* [Registration hooks](/community-mod-framework/registration-and-on-action-hooks.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmm.wiki
[2] Example mod `submods/cmf-example-mod`
