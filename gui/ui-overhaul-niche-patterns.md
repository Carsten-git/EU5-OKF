---
type: Reference
title: UI overhaul niche patterns
description: Low-priority Glorp patterns — alert cosmetics, dead suppress code, one-off compat hacks.
tags: [gui, niche, glorp, low-priority]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp patterns **#19 / #21 / #23** — valid know-how, rarely needed for content mods.

# Alert banner cosmetic (#19)

Override `template alert_banner_setup` (e.g. icon 30→35px) in a dedicated file. Does **not** replace CMF Custom Alerts — pure chrome on vanilla alert manager.

# Manual suppress dead code (#21)

Glorp ships unused `if = { always = no }` branches that touch GUI-only vars / callback flags. Prefer [`cmf_suppress`](/community-mod-framework/utility-triggers-and-effects.md) or [never-trigger-me](/gui/never-trigger-me-workaround.md). Do not copy Glorp’s uncalled suppress effects.

# One-off mod-compat (#23)

Example: left-panel tab `onclick` runs `GUI.ClearWidgets ideas_window` for Idea Variation compatibility. Document as a one-liner hack when integrating with a specific peer — not a general architecture.

# Citations

[1] Glorp `in_game/gui/glorpUI_alertmanager.gui`
[2] Glorp `in_game/common/scripted_effects/glorpui_cmm_warning_suppression.txt`
[3] Glorp `in_game/gui/panels/left_panel/glorpUI_left_panel.gui`
