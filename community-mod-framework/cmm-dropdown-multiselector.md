---
type: Reference
title: CMM dropdown multiselector
description: Enable multiselector UI on CMM dropdowns via cmm_set_dropdown_multiselector — Glorp ROI settings (pattern #28).
tags: [cmf, cmm, dropdown, gui, glorp]
timestamp: 2026-07-12T22:45:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#28**: after registering a CMM dropdown, call `cmm_set_dropdown_multiselector` so the menu renders the dropdown with **multiselector** interaction (CMF affordance for compact option picking in dense settings rows).

# Glorp usage

Both Build Panel ROI dropdowns:

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

cmm_register_dropdown_setting = {
	mod_id = glorpui
	setting_id = roi_unit
	tab_id = build_panel
	group_id = general
	default_index = 1
	option_count = 2
}
cmm_set_dropdown_multiselector = { setting = glorpui__roi_unit }
```

**Parameter:** `setting` = full CMM flag key `mod_id__setting_id` (not just `setting_id`).

Call **immediately after** the matching `cmm_register_dropdown_setting` in the same registration effect.

# Pair with option aliases

Glorp maps option 2 to GUI-friendly presence vars:

```txt
cmm_sync_dropdown_option_alias = {
	setting = glorpui__roi_metric
	index = 2
	alias = roiUseProfit
}
cmm_sync_dropdown_option_alias = {
	setting = glorpui__roi_unit
	index = 2
	alias = roiUseMonths
}
```

Re-sync in `cmf_on_callback` when `var:cmf_callback` matches the dropdown flag.

GUI panels then branch:

```txt
visible = "[Player.MakeScope.GetVariable('roiUseProfit').IsSet]"
visible = "[Player.MakeScope.GetVariable('roiUseMonths').IsSet]"
```

See [Script values and ROI in GUI](/gui/script-values-and-roi-in-gui.md).

# Loc for options

Per CMF convention:

```yaml
glorpui__roi_metric_option_1_name: "Tax income"
glorpui__roi_metric_option_2_name: "Profit"
glorpui__roi_unit_option_1_name: "Years"
glorpui__roi_unit_option_2_name: "Months"
```

Optional `_option_N_desc` tooltips.

# When to use

| Use multiselector dropdown | Use plain dropdown |
|----------------------------|-------------------|
| 2–4 mutually exclusive modes in a tight settings row | Long option lists with descriptions |
| ROI / display mode / AI aggressiveness presets | Settings lists with per-row fields |

Not required for dropdowns to function — it is a **presentation** hint to CMM.

# Related

* [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md)
* [CMM aliases for GUI](/community-mod-framework/cmm-aliases-for-gui.md)
* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)

# Citations

[1] Glorp `in_game/common/scripted_effects/glorpui_cmm_effects.txt` — `cmm_set_dropdown_multiselector`
[2] CMF wiki `cmm.wiki` — Dropdown, Setting Aliases
[3] Glorp `main_menu/localization/english/glorpui_cmm_l_english.yml` — ROI option labels
