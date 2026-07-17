---
type: Playbook
title: Integrating with Glorp UI
description: How content mods coexist with Glorp — load-order truth, merge strategy, and non-GUI fallbacks.
tags: [glorp, compatibility, gui, cmf, multi-mod]
timestamp: 2026-07-12T22:30:00+10:00
status: complete
source_mod: glorp.ui
---

Glorp pattern **#26**: UI overhauls win whole files. Content mods that ship a vanilla-copy `location_window.gui` **replace** Glorp’s location panel; they do not merge and rarely crash — they **hide** features or **revert** Glorp chrome.

# What players see today (typical)

| Load order winner | Glorp location UI | Content-mod button (e.g. Convert RGO) |
|-------------------|-------------------|----------------------------------------|
| Glorp below content mod | Broken / vanilla-revert | Maybe visible (wrong layout context) |
| Content mod below Glorp | Works | **Missing** (patched widget not in Glorp file) |

**Crashes are uncommon.** Failure mode is silent feature loss or ugly layout — not `ErrorDeviceLost` / script exceptions.

# Why vanilla-copy patches fail

1. **Last file wins** — one `in_game/gui/location_window.gui` per load order.
2. **Different widget tree** — Glorp removed the compact RGO `button_regular` row; RGO is `header_button_left` — see [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md).
3. **Different line count** — Glorp ~8.2k lines vs vanilla copy ~9.6k; patching vanilla does not port to Glorp.

# Integration strategies (pick one or combine)

## A — Glorp-merge submod (best in-panel UX for Glorp users)

1. Copy Glorp’s `location_window.gui` as base (pin Glorp version in submod readme).
2. Add minimal `# your_mod:` sibling button beside `name = "location_rgo"`.
3. Ship as optional submod or version-gated patch; **load after Glorp**.
4. Re-merge when Glorp bumps `glorpui_last_updated_game_version`.

**Pros:** Button where Glorp players look. **Cons:** Maintenance every Glorp release.

## B — CMF Action Bar / separate entry (best long-term compat)

Register `cmf_add_action_bar_element` (requires CMF dependency) or a small lateral view / generic action. No `location_window` override.

**Pros:** Works with Glorp and other UI overhauls. **Cons:** Less contextual — player picks location outside the province panel.

## C — Vanilla-only button + honest Workshop note (current v1)

Keep location button for non-overhaul users; document Glorp incompatibility; no merge.

**Pros:** Zero CMF dependency. **Cons:** Glorp subscribers cannot convert from UI.

## D — Peer widget type (ecosystem play)

If Glorp or CMF hosts a shared empty widget type (like `cm_auto_expand_rgo_widget_location_window`), a dependency could publish `rgo_conv_widget_location_window` for Glorp to instantiate. Requires **author cooperation** — not solo-friendly.

# Detection gates (optional polish)

Glorp detects peers via `cmf_active_mod_ids`:

```txt
glorpui_is_cm_active = {
	scope = country
	is_shown = {
		is_target_in_global_variable_list = {
			name = cmf_active_mod_ids
			target = flag:cm
		}
	}
}
```

Content mods can register `cmf_register_mod = { mod_id = your_mod }` and optionally skip shipping `location_window.gui` when `glorp.ui` is active — but **skipping the file** requires a separate entry path (strategy B), not detection alone.

# Recommended path for RGO Conversion–style mods

| Phase | Action |
|-------|--------|
| Now | Workshop note: Glorp incompatible for location button; gameplay scripts OK |
| v0.2 | CMF Action Bar fallback + keep vanilla button when CMF optional |
| v0.3+ | Optional `submods/glorp-compat/` merge patch; optional [expand raw goods](/gui/expand-raw-goods-lateralview.md) row button |
| Settings | [CMM catalog](/community-mod-framework/cmm-settings-catalog-design.md) for AI/cooldown toggles — [no lobby wizard](/community-mod-framework/player-options-without-lobby.md) |

# Testing checklist

1. Enable Glorp + your mod; Glorp **below** yours — open owned location: is your button present? Is Glorp chrome intact?
2. Reverse load order — button should disappear; confirm no crash on open/convert event via console `event` if needed.
3. With CMF fallback — convert flow works without any `location_window` override winning.

# Related

* [Glorp UI file inventory](/references/glorp-ui-file-inventory.md)
* [Construction Manager as reference](/references/construction-manager-as-reference.md) — peer `glorpui_is_cm_active` + shared types
* [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md)
* [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md)
* [Context-specific widget aliases](/gui/context-specific-widget-aliases.md)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md)
* [UI override file layering](/gui/ui-override-file-layering.md)
* [Action bar and alerts](/community-mod-framework/action-bar-and-alerts.md)
* [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md)

# Citations

[1] Glorp `in_game/gui/location_window.gui`
[2] RGO Conversion mod — vanilla-copy override lesson
[3] [Glorp UI as reference](/references/glorp-ui-as-reference.md)
