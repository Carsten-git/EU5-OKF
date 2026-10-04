---
type: Playbook
title: GitHub CI mod hygiene
description: PR-scoped UTF-8 BOM, LF line-ending, and changelog checks for EU5 mod repos — adapted from MnT-EU5.
tags: [tooling, ci, bom, encoding, github, validation]
timestamp: 2026-07-20T21:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

Large EU5 mods ship hundreds of `.txt` / `.yml` files. **Local** hygiene (`tools/BOM_finder/`) catches issues before push; **CI** enforces them on pull requests so Workshop uploads do not regress.

MnT-EU5 (GitHub `MEIOU-and-Taxes/MnT-EU5`) ships three workflows under `.github/workflows/`. Copy and adapt path prefixes to your mod tree.

# Three checks

| Workflow | What it enforces | When it runs |
|----------|------------------|--------------|
| `require-utf-8-bom-encoding.yml` | Changed script/loc files under configured prefixes must be UTF-8 **with BOM** | PR open/sync/reopen |
| `require-eol-lf.yml` | Changed text files must use **LF** only (no CRLF) | PR open/sync/reopen |
| `require-changelog.yml` | `Documentation/Change log.md` must change unless all commits are chore/ci/docs-style | PR (non-draft) |

Pair CI with [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md) and [Common pitfalls](/validation/common-pitfalls.md).

# UTF-8 BOM check (PR-scoped)

**Why PR-scoped:** full-tree scans are slow and noisy; only validate files touched in the PR.

**Key config:**

- `BOM_PATH_PREFIXES` — folders that require BOM (mirror EU5 `in_game/common`, `events`, `main_menu/localization`, `loading_screen`, etc.)
- `EXCLUDE_BOM_PATH_PREFIXES` — e.g. `main_menu/common/named_colors/`
- `TEXT_EXTS` — `.txt`, `.yml`, `.gfx`, `.lua`
- **Skip `.gui`** — GUI files do not support BOM in EU5

**Algorithm:**

1. `git fetch origin $GITHUB_BASE_REF`
2. `git diff --name-only origin/$base...HEAD`
3. For each changed text file under a BOM prefix: require `\xEF\xBB\xBF` and valid UTF-8 decode

Local mirror: `tools/BOM_finder/BOM_finder.py` + `BOM_exceptions.py`.

# LF line-ending check

Reject `\r\n` in changed text files (`.txt`, `.yml`, `.gui`, `.md`, `.json`, `.csv`, …). Set `core.autocrlf false` and `core.eol lf` in the workflow before the Python scan.

Windows contributors should use `.gitattributes` (`* text=auto eol=lf`) in the mod repo root.

# Changelog requirement

Skips when **every** commit message matches conventional prefixes:

```
build|chore|fix|ci|docs|style|refactor|perf|test
```

Otherwise the PR must modify `Documentation/Change log.md`. Keeps player-facing release notes aligned with gameplay PRs.

# Adoption checklist

1. Copy the three YAML files into `.github/workflows/`.
2. Replace `BOM_PATH_PREFIXES` with **your** mod paths (`in_game/`, `main_menu/`, `loading_screen/` — not vanilla `game/` unless you vendor game files).
3. Add `tools/BOM_finder/` for pre-push local runs.
4. Document the changelog path in your mod `README.md`.
5. On failure, fix encoding locally and re-push — do not disable hooks for script/loc files.

# Related

* [Total conversion toolchain](/tooling/total-conversion-toolchain.md) — BOM finder, shared config
* [Error log cleaner](/tooling/error-log-cleaner-rotation.md) — complementary debug loop
* [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md) — GitHub source repo

# Citations

[1] MnT-EU5 `.github/workflows/require-utf-8-bom-encoding.yml`
[2] MnT-EU5 `.github/workflows/require-eol-lf.yml`
[3] MnT-EU5 `.github/workflows/require-changelog.yml`
[4] MnT-EU5 `tools/BOM_finder/`
