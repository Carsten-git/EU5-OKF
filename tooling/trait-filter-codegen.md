---
type: Playbook
title: Trait filter codegen
description: Generate character search filters and scripted triggers from vanilla trait data instead of hand-maintaining OR lists.
tags: [tooling, codegen, filters, characters, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#11**: extract the **pipeline**, not the generated trait dump (version-fragile).

# Pipeline

1. Script scans vanilla traits/modifiers (Glorp: `tools/glorpui_miscellaneous_updater.py` — may live in upstream repo, not always in Workshop folder).
2. Emits `scripted_triggers` (e.g. `glorpui_generated_trait_scripted_triggers.txt`).
3. Emits GUI filter entries (`in_game/gui/filters/glorpui_character.txt`).
4. Emits loc keys `search_filter_glorpui_*`.

Regenerate on game version bumps; never hand-edit the OR-block contents.

# Related

* [Total conversion toolchain](/tooling/total-conversion-toolchain.md) — same codegen mindset
* [Character interaction pre-evaluation](/interactions/character-interaction-pre-evaluation.md)

# Citations

[1] Glorp `in_game/gui/filters/glorpui_character.txt`
[2] Glorp `in_game/common/scripted_triggers/glorpui_generated_trait_scripted_triggers.txt`
[3] Glorp `main_menu/localization/english/glorpui_filters_l_english.yml`
