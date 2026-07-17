---
type: Playbook
title: UI override file layering
description: Structure large GUI overhauls — shared/, panels/, type redefs, cmfg extracts; know the cost of full-file copies and HUD type shadowing.
tags: [gui, overrides, architecture, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp patterns **#8 / #20 / #22**.

# File layering (#8)

| Layer | Role | Example |
|-------|------|---------|
| Vanilla-path override | Same relative path as vanilla | `economy_lateralview.gui` |
| Type redefinition file | Shadows `type topbar` etc. | `glorpUI_hud_topbar.gui` |
| Shared tooltips/types | Cross-cutting | `gui/shared/glorpUI_*.gui` |
| Panels | Sub-chrome | `gui/panels/left_panel/` |
| New lateral views | Extra windows | `glorpUI_people_lateral_view.gui` |
| Extracts | Types/templates only | `gui/vanilla/cmfg_*.gui` |

Mark edits with `# GlorpUI:` (or your prefix) for patch diffs.

# HUD type shadowing (#20)

Keep vanilla `ingame_topbar.gui` shell; redefine `type topbar` / mapmode window types in separate files that win type resolution. High maintenance on game patches — last resort for full HUD overhauls.

# Full-file override cost (#22)

Copying multi-thousand-line `location_window.gui` / `outliner_entries.gui` works with `cmfg_*` extracts reducing inner-type duplication, but every vanilla UI patch is a merge. Prefer CMM/scripted GUI / widget embedding when possible; treat full copies as architecture of last resort.

# Adding a sibling button (RGO Conversion lesson)

When injecting a button next to vanilla chrome:

* Prefer **`visible = "[Location.GetOwner.IsPlayer]"`** (or similar GUI owner check).
* Do **not** hide the widget with `visible = "[GetScriptedGui('…').IsShown(…)]"` unless you have verified IsShown stays true for owned locations — a false IsShown removes the button entirely (KI-063).
* Use ScriptedGui mainly for **`Execute`**, and **`IsValid` for greying** (`enabled = "[GetScriptedGui('…').IsValid(GuiScope…)]"`). Keep `visible` on a simple owner/context check — not IsShown.
* Example (RGO Conversion): button stays visible for player-owned locations; greys out when cooldown / busy / peasants gates fail via `is_valid`.
* Full pattern: [ScriptedGui IsValid button greying](/gui/scripted-gui-isvalid-greying.md).
* Glorp moves RGO to `header_button_left` — vanilla row injection misses Glorp users: [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md), [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md).

# See also

* [Known issues](/validation/known-issues.md) — KI-063, KI-068
* [ScriptedGui IsValid button greying](/gui/scripted-gui-isvalid-greying.md)

# Related

* [CMFG vanilla-type extraction](/gui/cmfg-vanilla-type-extraction.md)
* [Custom UI patterns](/gui/custom-ui-patterns.md)
* [Known issues](/validation/known-issues.md) — KI-063

# Citations

[1] Glorp `in_game/gui/` tree — `shared/`, `panels/`, `vanilla/`, `glorpUI_hud_*.gui`
[2] Glorp comments `# GlorpUI:` in override files
