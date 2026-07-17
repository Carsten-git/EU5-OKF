---
type: Playbook
title: Reading vanilla examples
description: How to grep, diff, and learn syntax from the EU5 game install before writing mod content.
tags: [getting-started, vanilla, workflow, grep]
timestamp: 2026-07-06T08:45:00+10:00
resource: game/
status: complete
---

EU5 modding is copy-from-working-examples. The game install is the canonical syntax reference — wikis and this bundle explain *where* to look; vanilla files show *how*.

# Set paths

| Name | Typical Windows path |
|------|----------------------|
| **Vanilla (GAME_ROOT)** | `C:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game` |
| **Your mod** | `Documents/Paradox Interactive/Europa Universalis V/mod/your_mod/` |
| **Logs** | `Documents/Paradox Interactive/Europa Universalis V/logs/` |

Point tools at `GAME_ROOT` — see [Vanilla file locations](/references/vanilla-file-locations.md) for folder map.

# Find an example of an effect or trigger

Use ripgrep (`rg`) from PowerShell or Cursor terminal:

```powershell
# Does vanilla use this effect name?
rg "trigger_event_non_silently" "$env:GAME_ROOT\in_game" --glob "*.txt"

# Who defines add_country_modifier with mode = add_and_extend?
rg "mode = add_and_extend" "$env:GAME_ROOT" --glob "*.txt"

# Country advances for a tag
rg "has_or_had_tag = TEU" "$env:GAME_ROOT\in_game\common\advances"

# Localization key pattern
rg "teu_crusader_discipline" "$env:GAME_ROOT\main_menu\localization"
```

Narrow by folder when you know the system:

| System | Grep in |
|--------|---------|
| Events | `in_game/events/` |
| On-actions | `in_game/common/on_action/` |
| Advances | `in_game/common/advances/` |
| Modifiers | `main_menu/common/static_modifiers/` |
| Missions | `in_game/common/missions/` |

# Read the schema stub

Many vanilla `common/` folders include `____Info.txt` or `readme.txt` with commented field lists. Example: `in_game/common/missions/____Info.txt`.

# Diff mod against vanilla

When overriding or extending vanilla content, diff helps spot unintended changes:

```powershell
# Compare one file (mod vs vanilla) — vanilla TEU advances
fc /N `
  "$env:GAME_ROOT\in_game\common\advances\country_TEU.txt" `
  "Documents\Paradox Interactive\Europa Universalis V\mod\northern_crusade_teu\in_game\common\advances\count_TEU_northern_crusade.txt"
```

For parallel **new** files (mod-only advances), skip diff — instead grep vanilla for the same **pattern** (e.g. another `country_HUN.txt`).

Git diff works when your mod is a repo:

```bash
git diff -- in_game/common/advances/
```

# Recommended read order for a new feature

1. [Vanilla file locations](/references/vanilla-file-locations.md) — pick folder
2. Grep vanilla for the mechanic keyword
3. Open the **closest country/system** example (TEU for Teutonic modding, HUN for medium DHE flavor)
4. Copy structure; rename ids with your mod prefix
5. Grep vanilla loc for key naming (`advances_l_english.yml`, `events/DHE/`)
6. Run [validate_mod.py](/validation/mod-validation-tooling.md) if available
7. Smoke test — [checklist](/validation/smoke-testing-checklist.md)

# IDE / Cursor tips

- Open workspace with **both** `game/` and `mod/` folders for cross-reference
- Use "Find in Files" limited to `in_game/common/advances` to avoid noise
- Follow `[Citations]` blocks in OKF articles — they point to exact vanilla paths

# Anti-patterns

- Guessing modifier keys without grepping `modifier_type_definitions`
- Copying EU4 syntax — always confirm in EU5 vanilla
- Reading only the mod's own files (no vanilla baseline)

# See also

* [Workflow and tools](workflow-and-tools.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)
* [Error log debugging](/validation/error-log-debugging.md)
* [Glossary](/references/glossary.md)

# Citations

[1] Vanilla install: `C:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\` — canonical syntax reference (GAME_ROOT)
[2] OKF: [Vanilla file locations](/references/vanilla-file-locations.md) — folder map for `in_game/` and `main_menu/`
[3] Vanilla: `game/in_game/common/missions/____Info.txt` — schema stub with commented field lists
[4] Vanilla: `game/in_game/common/advances/country_TEU.txt` — country-specific advance example
[5] Mod: `mod/northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt` — mod override / parallel file diff target
[6] Mod: `mod/northern_crusade_teu/tools/validate_mod.py` — structural validation before launch
[7] Logs: `Documents/Paradox Interactive/Europa Universalis V/logs/error.log`
