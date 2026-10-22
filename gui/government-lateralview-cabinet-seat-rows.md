---
type: Playbook
title: Government lateral view — cabinet-style seat rows
description: Add a Country sub-tab with per-location seat cards, appoint bar, and scripted generic actions (Control reworked Local Authorities / Bailiwick stewards).
tags: [gui, government, cabinet, lateralview, generic-actions, scripted-gui]
timestamp: 2026-10-22T23:45:00+11:00
status: complete
source_mod: control_reworked
---

Reference implementation: **Control reworked** REQ-002 (`mod/Control reworked/in_game/gui/government_lateralview.gui`, steward types in the same file under `types CountryHeaders`).

# Goal

Mirror **Government → Country → Cabinet**: one card per logical seat, location in the header, portrait + ability strip in the card body, and a **31px** `card_header_button_cabinet` row for appoint / character name. Wire clicks to a **generic action** with a fixed `target_location` parameter.

# Country sub-tab (fourth tab pattern)

1. Add a lateral var, e.g. `control_reworked_gov_subtab = local_authorities`, cleared when other Country subtabs open.
2. Use `automated_tab` with `checkbox_visible = no` and `blockoverride "tab_text"` for the label (not raw `text` on a plain tab).
3. On tab open, call a scripted GUI that refreshes a **country variable list** of locations (do not rebuild that list on every appoint/dismiss — it breaks datamodel bindings until the tab is reopened).

```txt
# scripted_guis — refresh list (country scope)
control_reworked_refresh_bailiwick_list = {
	scope = country
	effect = { control_reworked_refresh_bailiwick_location_list = yes }
}
```

List binding:

```gui
datamodel = "[GovernmentView.GetPlayer.MakeScope.GetList('control_reworked_bailiwick_locations')]"
item = {
	vbox = {
		datacontext = "[Scope.GetLocation]"
		...
	}
}
```

Build the list in a scripted effect with `save_scope_as` + `root = { add_to_variable_list { target = scope:... } }` so entries land on the **country**, not inside `every_owned_location`.

# Seat card + appoint bar layout

**Do not** reuse `cabinet_character_button` without `Cabinet` datacontext — vanilla text falls back to `ADD_CABINET_CHARACTER`.

**Do** copy `cabinet_character_button` styling from `in_game/gui/shared/cabinet_cards.gui` into a custom type extending `card_header_button_cabinet`, with `Location` bindings.

**Layout pitfall:** a separate 31px row **below** `bg_cabinet_card_frame` may not paint in scroll lists. Control reworked keeps the appoint bar **inside** the seat `vbox` (120px card widget + steward button as second child). That matches player-visible cabinet chrome even though vanilla places the button outside the card widget.

```gui
type control_reworked_bailiwick_seat_card = vbox {
	control_reworked_bailiwick_seat_card_inner = widget { size = { -1 120 } ... }
	control_reworked_steward_character_button = {}
}
```

Define steward UI types in **`government_lateralview.gui`** (inside `types CountryHeaders`) so they load with the lateral view override.

# Generic action on the appoint bar

Action: `show_in_gui_list = no` is fine for dedicated buttons. Per `in_game/common/generic_actions/readme.txt`:

- Use `scripted_action_tooltip` with `parameter = { parameter_name = "target_location" parameter_value = "[Location.MakeScope]" }`.
- For a **always-visible** bar (show name when staffed), set `visible = yes` and `enabled = yes` on the button. Tying `visible` to `[PdxGuiWidget.IsUberButtonVisible]` without an uber header parent **hides** the row.
- Optional header: clone Cabinet’s `hire_advisor` `header_action_button_left_uber` block for Hire Courtier.

Replace flow: `control_reworked_apply_steward_at_location` clears then sets; allow `control_reworked_appoint_steward_at_bailiwick` on any staffable Bailiwick, not only vacant posts.

# Header content

Cabinet inactive seats show a red `NO_CABINET_ACTION` banner. For location-based seats, show **only** location (yellow) + province subtitle — no vacant/staffed labels in the header.

# Checklist

| Check | |
|-------|---|
| UTF-8 BOM on `.gui` / `.yml` | `tools/apply_utf8_bom.py` |
| `STATIC_MODIFIER_NAME_*` / `STATIC_MODIFIER_DESC_*` for new modifiers | loc mirrors `main_menu` + `in_game` |
| Full game restart after GUI changes | hot reload unreliable |
| `python tools/run_automated_tests.py` | mod validators |
