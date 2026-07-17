---
type: Playbook
title: Cross-mod GUI integration
description: Detect CMF peer mods via cmf_active_mod_ids, gate widgets, and embed peer-mod GUI types as black boxes.
tags: [cmf, gui, multi-mod, compatibility, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp patterns **#2 / #7 / #24**: optional UI integration with other CMF-ecosystem mods without hardcoding their internals.

# Detect peer mod (#2)

```txt
glorpui_is_cm_active = {
	scope = country
	is_shown = {
		is_target_in_global_variable_list = {
			name = cmf_active_mod_ids
			target = flag:cm   # peer mod's registered id
		}
	}
	effect = { }
}
```

In `.gui`:

```txt
visible = "[Not(GetScriptedGui('glorpui_is_cm_active').IsShown(GuiScope.SetRoot(Player.MakeScope).End))]"
# hide vanilla control when peer active
```

# Embed peer widgets (#7)

After the detection gate, instantiate empty types defined by the dependency:

```txt
# Conceptual — types come from Construction Manager / SmartTaxes / AT
smart_taxes_replacement_enable_checkbox = {}
cm_auto_expand_rgo_widget = {}
atd_outliner_sync_widget = {}
```

Hide conflicting vanilla checkboxes when the peer is active. Do not reimplement their logic.

# Shared cross-mod types (#24)

Publish reusable types in a shared file (Glorp: `types glorp_shared_types` with comments like “Shared with Construction Manager”) so peer mods can call the same widget by name.

# Related

* [Mod detection](/community-mod-framework/utility-triggers-and-effects.md)
* [Custom UI patterns](/gui/custom-ui-patterns.md)
* [Glorp UI reference](/references/glorp-ui-as-reference.md)

# Citations

[1] Glorp `in_game/common/scripted_guis/glorpui_construction_manager_scripted_gui.txt`
[2] Glorp `in_game/common/scripted_guis/glorpui_smart_taxes_scripted_gui.txt`
[3] Glorp `in_game/gui/economy_lateralview.gui`, `expand_raw_goods_lateralview.gui`, `outliner_entries.gui`
[4] Glorp `in_game/gui/glorpUI_shared_types.gui`
