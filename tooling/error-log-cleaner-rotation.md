---
type: Playbook
title: Error log cleaner and rotation
description: Dedupe error.log entries, tag new lines, and rotate cleaned logs for iterative EU5 balance debugging.
tags: [tooling, error-log, debugging, telemetry, balance]
timestamp: 2026-07-20T21:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

Long observer runs produce megabyte `error.log` files where the same binding failure or telemetry line repeats thousands of times. MnT's `tools/log_cleaner/process_log.py` turns raw logs into a **diff-friendly** artifact for balance work.

Complements [Error log debugging](/validation/error-log-debugging.md) and delimiter telemetry in [Total conversion toolchain](/tooling/total-conversion-toolchain.md). For structured pick/market CSV exports (Sire-style), see [RGO balance telemetry pipeline](/validation/rgo-balance-telemetry-pipeline.md).

# What it does

1. **Rotate** three cleaned files: `cleaned_error.log` → `_old` → `_oldest` (delete oldest).
2. **Strip** `[HH:MM:SS]` timestamps from each line.
3. **Group** multi-line entries by `[file.cpp:N]` header (new entry starts when that pattern appears).
4. **Dedupe** identical entries; prefix with `COUNT:n`.
5. **Tag new** entries not present in the previous `_old` file with `!! NEW !!`.
6. **Truncate** entries longer than three lines (marks `(entry shortened)`).

# Config

Uses `tools/shared/config.ini` via `fetch_logs.get_from_config('Paths', 'log_directory')`:

```ini
[Paths]
log_directory = C:\Users\...\Documents\Paradox Interactive\Europa Universalis V\logs\
```

Run with cwd under `tools/` (see `tools/readme.txt`).

# Typical loop

```text
Play session → error.log grows
  → python tools/log_cleaner/process_log.py
  → read cleaned_error.log (sorted by COUNT, NEW lines highlighted)
  → fix script/loc
  → repeat
```

Pair with `tools/plot/log_parser.py` + `MT_grapher.py` when lines use `::GP::` / `::MK::` prefixes ([data-binding macros](/tooling/data-binding-macros.md)).

# When to use vs other tools

| Tool | Best for |
|------|----------|
| `process_log.py` | Finding **new** errors after a change; deduping spam |
| `log_parser.py` + grapher | **Time series** from `::TG::` / `::GP::` telemetry |
| Sire `export_sire_*` scripts | Structured **AI pick / market** CSV from `SIRE_*` lines |
| Vanilla grep | One-off search in raw `error.log` |

# Related

* [GitHub CI mod hygiene](/tooling/github-ci-mod-hygiene.md)
* [Script telemetry via hidden events](/validation/script-telemetry-via-hidden-events.md)

# Citations

[1] MnT-EU5 `tools/log_cleaner/process_log.py`
[2] MnT-EU5 `tools/log_cleaner/readme_process_log.txt`
[3] MnT-EU5 `tools/shared/fetch_logs.py`
