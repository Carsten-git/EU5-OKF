---
type: Reference
title: Multi-file localization split for UI mods
description: Split large UI mod loc by concern and language; stub keys to silence IDE undefined-key warnings.
tags: [localization, ui, organization, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#17**:

| File pattern | Content |
|--------------|---------|
| `glorpui_cmm_l_<lang>.yml` | CMM setting/tab/group keys |
| `glorpui_shared_l_<lang>.yml` | Shared UI strings |
| `glorpui_filters_l_<lang>.yml` | Character filter labels |
| `glorpui_cmm_warning_suppression_l_<lang>.yml` | Stubs for missing CMM pause-menu keys |
| `glorpui_l_<lang>.yml` | HUD / misc |

Self-referencing flag keys (e.g. `glorpui: "glorpui"`) at the bottom of CMM loc can suppress IDE “undefined key” noise for flag names used as loc keys.

Ship all languages you care about in parallel folders under `main_menu/localization/`.

# Related

* [Localization key conventions](/localization/localization-key-conventions.md)
* [UTF-8 BOM](/localization/utf8-bom-requirement.md)

# Citations

[1] Glorp `main_menu/localization/<lang>/glorpui_*.yml`
