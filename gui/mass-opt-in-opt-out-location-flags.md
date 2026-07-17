---
type: Playbook
title: Mass opt-in / opt-out location flags
description: Country mass-enable flag flips location variable semantics — default all on with exclusion vars, or default off with opt-in vars — shared by UI, filters, and automation.
tags: [gui, variables, ux, automation, rgo]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

Construction Manager pattern **CM-4**: **dual-mode** per-feature toggles so players can either opt locations in one-by-one or flip a country **mass** switch and only exclude exceptions.

# Problem

“Enable auto-expand on 200 locations” via individual clicks is unusable. A single global bool loses per-location exceptions.

# Semantics

| Mode | Country signal | Location “on” means |
|------|----------------|---------------------|
| **Normal** | mass flag **absent** | `exists = var:<feature>` (opt-in) |
| **Mass** | mass flag **present** | feature on **unless** `exists = var:<feature>_excluded` |

CM RGO example:

- Opt-in: `var:cm_auto_expand_rgo`
- Mass: country `var:cm_mass_auto_expand_rgo`
- Opt-out under mass: `var:cm_auto_expand_rgo_excluded`

Same shape for auto-food and building auto-expand lists.

# Eligibility (shared)

```txt
OR = {
	exists = var:cm_auto_expand_rgo
	AND = {
		owner = { exists = var:cm_mass_auto_expand_rgo }
		NOT = { exists = var:cm_auto_expand_rgo_excluded }
	}
}
```

Use this expression in:

- scripted_gui `IsShown` / toggle effects
- monthly automation triggers
- [search filters](/gui/search-filters-for-lists.md)

# UI

- Per-row checkbox toggles opt-in **or** exclusion depending on mass mode.
- Header **mass** button sets/clears the country mass flag (CM also uses shift-click + hidden datamodels to bulk-toggle **visible filtered** rows — advanced; see CM `cm_mass_auto_expand_*`).
- Right-click → CMM settings via shared template (`cm_rightclick_open_settings_window`).

# Reusable lesson

One semantic definition, three consumers (button, pulse, filter). Never let filters drift from automation triggers.

# Related

* [Search filters for lists](/gui/search-filters-for-lists.md)
* [Off-screen scripted widget drivers](/gui/offscreen-scripted-widget-drivers.md)
* [Mass-action UI counter](/gui/mass-action-ui-counter.md) — Glorp batch recruit (different)

# Citations

[1] CM `in_game/common/scripted_guis/cm_rgos.txt`
[2] CM `in_game/gui/filters/cm_location.txt` — `location_has_auto_expand_rgo`
[3] CM `in_game/common/scripted_triggers/cm_rgo_triggers.txt`
