---
type: Reference
title: Mod validation tooling
description: validate_mod.py pattern — manifest checks, modifier keys, advances, triggers, loc coverage.
resource: mod/northern_crusade_teu/tools/validate_mod.py
tags: [validation, tooling, python]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Automated checks catch structural mistakes before launching EU5. The reference implementation lives in `northern_crusade_teu/tools/validate_mod.py` — adapt it for any mod with a stable prefix and manifest.

# Quick start

```bash
cd northern_crusade_teu
python tools/validate_mod.py --all
python tools/validate_mod.py --modifiers --advances --loc
```

Exit code `0` prints `OK — scanned N mod files, M modifier types loaded`. Exit code `1` lists every failure line.

# Architecture

```
validate_mod.py
├── load_modifier_types()     ← parses vanilla 00_modifier_types.txt
├── collect_mod_files()       ← rglob in_game/ + main_menu/ for .txt/.yml
├── check_manifest_files()    ← content_manifest.json done-files exist
├── check_modifiers()         ← teu_nc_* static modifier stat keys
├── check_advances()          ← teu_nc_* advance stat keys
├── check_design_advance_modifiers()  ← DESIGN.md §7 key scan
├── check_triggers()          ← scripted trigger file presence + banned patterns
├── check_loc_coverage()      ← event title keys have yml entries
└── check_duplicate_keys()    ← no duplicate teu_nc_* / event ids
```

# Vanilla modifier type source

The script reads the authoritative key list from the game install:

```
game/main_menu/common/modifier_type_definitions/00_modifier_types.txt
```

Each line `stat_key = {` registers a valid modifier/advance bonus key. Set `GAME_ROOT` in the script to your Steam path (or make it an env var when porting).

# Key validation logic

For each modifier/advance block, the script:

1. Skips structural keys (`ADVANCE_SKIP_KEYS`: `age`, `potential`, `requires`, `unlock_*`, …).
2. Checks remaining `key = value` pairs against `valid_types`.
3. Allows numeric literals and known suffix patterns (`_bonus`, `societal_value_*`, `estate_satisfaction_*`).

Unknown keys emit:

```
[advance] count_TEU_northern_crusade.txt::teu_nc_foo: unknown modifier key 'typo_discipline'
[modifier] teu_nc_purpose_meter: unknown modifier key 'monthly_foo'
```

# CLI flags

| Flag | Checks |
|------|--------|
| `--all` | Everything (default when no flags given) |
| `--manifest` | Manifest done-files + duplicate keys |
| `--modifiers` | `main_menu/common/static_modifiers/` |
| `--advances` | `in_game/common/advances/` + DESIGN.md §7 |
| `--triggers` | `scripted_triggers/teu_nc_triggers.txt` |
| `--loc` | Event title keys vs yml content |

# Adapting for your mod

1. Copy `validate_mod.py` to `your_mod/tools/`.
2. Change `MOD_ROOT`, prefix regex (`teu_nc_` → `yourprefix_`), and `GAME_ROOT`.
3. Trim checks you do not need (triggers, DESIGN scan).
4. Wire into CI or pre-commit: `python tools/validate_mod.py --all || exit 1`.

Related scripts in the same mod:

| Script | Role |
|--------|------|
| `validate_manifest.py` | Progress report from `content_manifest.json` |
| `sanity_check_design.py` | Banned patterns in DESIGN.md |
| `pre_implementation_check.py` | Sprint gate before implementation |
| `generate_localization.py` | Keeps loc in sync (reduces `--loc` failures) |

# Limits

The validator does **not** replace in-game testing:

- Trigger syntax validity (only banned-pattern scan)
- Scope correctness in events
- UTF-8 BOM on yml (use editor settings + [BOM article](/localization/utf8-bom-requirement.md))
- Load order / override conflicts

Pair with [error log debugging](error-log-debugging.md) after launch.

# See also

* [Workflow and tools](/getting-started/workflow-and-tools.md)
* [Modifier stat keys](/modifiers/modifier-stat-keys.md)
* [Smoke testing checklist](smoke-testing-checklist.md)

# Citations

[1] `northern_crusade_teu/tools/validate_mod.py`
[2] `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt`
