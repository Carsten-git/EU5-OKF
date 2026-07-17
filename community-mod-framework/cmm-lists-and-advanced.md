---
type: Playbook
title: CMM lists and advanced settings
description: Settings lists, text inputs, aliases, non-resettable flags, and conditional visibility for CMM.
tags: [cmf, cmm, settings-list, advanced]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

Most CMM types auto-apply. **Text** and **settings lists** need callbacks. Global settings use `cmm_register_global_*`.

# Text (singleplayer only)

```txt
cmm_register_text_setting = {
	mod_id = your_mod
	setting_id = your_text
	tab_id = general
	group_id = general_values
	character_limit = 42
	quote_text = 1
}
```

Callback is a **scripted effect** `your_mod__your_text_on_changed` with `$text$`.

# Settings list

```txt
cmm_register_settings_list = {
	mod_id = your_mod
	setting_id = your_list
	tab_id = general
	item_count = 5    # 1–50
	is_ordered = 1
}
```

Then register fields: `cmm_register_list_bool_field`, `_dropdown_field`, `_numeric_field`, `_slider_field`, `_data_field` (read-only; set with `cmm_set_list_data_value`).

Required Scripted GUI callback:

```txt
your_mod__your_list_on_changed = {
	scope = country
	effect = {
		cmm_apply_list_change = { setting = your_mod__your_list }
	}
}
```

**Ordering:** stable item id `1..N` never changes; reorder only changes display order. `cmm_for_each_list_item` visits display order; `$i$` is the stable id.

Field storage: `"variable_map(cmm|flag:[mod]__[list]_i[N]_f[slot])"`.

Dynamic lists: `cmm_begin_settings_list` → `cmm_add_settings_list_item` → `cmm_finish_settings_list`, or `cmm_register_settings_list_from_list` from a country variable list (max 50). Attach scopes with `cmm_set_list_item_value` → `scope:cmm_list_current_item_value` while iterating.

# Aliases & reset

```txt
cmm_sync_setting_alias = { setting = your_mod__your_toggle alias = your_mod_toggle }
cmm_sync_bool_alias = { … }          # has_variable pattern
cmm_sync_bool_alias_inverted = { … } # “disable X” labels — see catalog design
cmm_sync_dropdown_option_alias = { setting = … index = 2 alias = using_feature_b }
cmm_set_no_reset = { mod_id = your_mod setting_id = your_setting }
```

Optional dropdown UI: [CMM dropdown multiselector](/community-mod-framework/cmm-dropdown-multiselector.md) (#28).

Call alias sync from registration **and** `cmf_on_callback`.

# Conditional visibility

Register a Scripted GUI named after the element with `cmf_register_scripted_gui = { element = … }`. For CMM settings, `is_shown` on `[mod]__[setting]_on_changed`. Action bar: `is_shown` / `is_valid`.

# Unrestricted tools gate

`cmm_set_requires_unrestricted_tools_enabled` after registration — menu greys out unless host enables unrestricted tools. See [utilities](/community-mod-framework/utility-triggers-and-effects.md).

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmm.wiki — Advanced / Settings List
[2] CMF wiki: Conditional Visibility, Reading Setting Values
