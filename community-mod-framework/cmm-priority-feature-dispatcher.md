---
type: Playbook
title: CMM priority feature dispatcher
description: Ordered CMM list of feature flags drives a monthly switch — post-registration builds fast lookup lists from CMM storage.
tags: [cmf, cmm, on-actions, automation, lists]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

Construction Manager pattern **CM-5**: expose an **ordered priority list** in CMM, then each monthly pulse walks enabled features in that order.

Extends [CMM lists and advanced](/community-mod-framework/cmm-lists-and-advanced.md) with a full worked dispatcher.

# Problem

Many automations compete for gold each month. Players need to **reorder** and **disable** features without editing script; monthly code needs **fast** lookups, not raw CMM map walks every time.

# Registration (sketch)

1. `cmm_register_*` for mod `cm` with tab `settings` and ordered list `priority_list`.
2. Items are **flags** (`flag:cm_feature_auto_expand_rgos`, …).
3. After registration, `cm_cmm_post_registration` builds runtime structures via `cmm_build_list_ordered_values`, `cmm_build_list_field_map`, `cmm_build_list_bool_list`.
4. Sync aliases (`cmm_sync_setting_alias`) so gameplay reads `cm_minimum_gold` instead of `cm__minimum_gold`.

Hook: `cmf_on_mod_registration` → register + post-registration; `cmf_on_callback` for list changes → rebuild lookups.

# Monthly dispatch

```txt
every_in_list = {
	variable = cm_priority_features_list
	save_scope_as = cm_feature
	if = {
		limit = {
			scope:cm_country = {
				is_target_in_variable_list = {
					name = cm_priority_features_enabled
					target = scope:cm_feature
				}
			}
		}
		switch = {
			trigger = scope:cm_feature
			flag:cm_feature_auto_expand_buildings = { scope:cm_country = { cm_run_auto_expand_buildings = yes } }
			flag:cm_feature_auto_expand_rgos = { scope:cm_country = { cm_run_auto_expand_rgos = yes } }
			# …
		}
	}
}
```

Gate with pause setting (`cm_should_pause_auto_expand`) before the loop.

# Extra CM techniques worth copying

| Technique | Why |
|-----------|-----|
| Per-metric threshold memory | Changing expand metric dropdown saves/restores separate threshold columns |
| `cmm_hide_list_item` / show | Hide buildings/goods not yet available |
| List migration | If stored item count < expected, clear init flags and re-register |
| Global bool setting | e.g. free town-rights grants applied to every country |

# Related

* [CMM lists and advanced](/community-mod-framework/cmm-lists-and-advanced.md)
* [Player options without lobby](/community-mod-framework/player-options-without-lobby.md)
* [Off-screen scripted widget drivers](/gui/offscreen-scripted-widget-drivers.md) — staging after dispatch
* [Construction Manager as reference](/references/construction-manager-as-reference.md)

# Citations

[1] CM `in_game/common/scripted_effects/cm_cmm_effects.txt`, `cm_cmm_custom_effects.txt`
[2] CM `in_game/common/on_action/cm_cmm_on_actions.txt`, `cm_on_action.txt`
[3] CM `in_game/common/scripted_guis/cm_cmm_scripted_gui.txt`
