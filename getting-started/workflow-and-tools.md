---
type: Playbook
title: Workflow and tools
description: Suggested dev loop — validate, generate localization, grep vanilla.
tags: [getting-started, tooling, workflow]
timestamp: 2026-07-06T00:00:00+10:00
resource: mod/northern_crusade_teu/tools/
status: complete
---

# Recommended loop

1. **Read vanilla** — grep the game install for working examples ([reading vanilla](/getting-started/reading-vanilla-examples.md), [vanilla paths](/references/vanilla-file-locations.md)).
2. **Implement** in `in_game/` + `main_menu/localization/`.
3. **Generate loc** — scripts that emit UTF-8 BOM yml reduce hand-editing errors.
4. **Validate** — `validate_mod.py` for modifier/advance keys, manifest, loc coverage ([tooling](/validation/mod-validation-tooling.md)).
5. **Smoke test in-game** — [checklist](/validation/smoke-testing-checklist.md).

# Patterns that scale

| Pattern | Why |
|---------|-----|
| `tools/generate_localization.py` | Keeps `STATIC_MODIFIER_*` keys in sync with modifier defs |
| `tools/generate_event_localization.py` | One yml per event script file + `.entry` keys for DHE |
| `tools/validate_mod.py` | Catches structural mistakes before launch |
| `data/content_manifest.json` | Optional manifest of expected loc files |

Example implementation: `northern_crusade_teu/tools/` in the same `mod/` folder.

# See also

* [Error log debugging](/validation/error-log-debugging.md)
* [Common pitfalls](/validation/common-pitfalls.md)

# Citations

[1] Mod: `mod/northern_crusade_teu/tools/validate_mod.py` — modifier/advance key and manifest checks
[2] Mod: `mod/northern_crusade_teu/tools/generate_localization.py` — UTF-8 BOM yml generation for static modifiers
[3] Mod: `mod/northern_crusade_teu/tools/generate_event_localization.py` — per-event-script yml + `.entry` keys
[4] Mod: `mod/northern_crusade_teu/data/content_manifest.json` — expected loc file manifest
[5] OKF: [Mod validation tooling](/validation/mod-validation-tooling.md)
[6] OKF: [Reading vanilla examples](reading-vanilla-examples.md) — grep-first workflow against GAME_ROOT
