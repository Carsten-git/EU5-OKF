---
type: Playbook
title: Total conversion toolchain
description: Shared config, BOM checks, template codegen, and error_log telemetry for EU5 overhaul development.
tags: [tooling, bom, codegen, telemetry, balance]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Before a total conversion scales content, industrialize the **dev loop**. MnT ships a `tools/` tree that is not required to play but encodes how the team balances and debugs.

# Shared config

One `tools/shared/config.ini` (from `example_config.ini`) for:

- `game_directory`, `log_directory`, `save_directory`
- Autosave interval / watcher options

Scripts should fail fast with copy instructions if config is missing. Run with cwd under `tools/` (or sibling of `shared/`).

# Encoding: BOM finder

EU5 expects UTF-8 **with BOM** on many `.txt` / `.yml` files. Automate:

- Walk the mod (or game) tree
- Report folders below an 80% BOM compliance threshold
- Pair with CI encoding/line-ending checks in the source repo

See also [Common pitfalls](/validation/common-pitfalls.md) and [Localization BOM](/localization/).

# Template codegen (`$GOOD$`)

For N-per-good buildings/PMs:

1. Templates in `input_templates/`
2. Parser scans vanilla/mod goods for `raw_material`
3. Expand placeholders → `output/` committed or generated in CI

Lightweight **brace-matching** parsers beat a full DSL until you need validation.

# Telemetry via `error_log`

No native time-series export → define a delimiter protocol:

```
::TG::year:tag:metric1:metric2:...
::GP::...   # goods prices
::MK::...   # markets
```

Enable with orphan debug events; fire yearly from one coordinator country. External Python (`log_parser` + grapher) charts the series. Document **column order** next to the parser.

For scoped picks (tag, loc, goods, `price_in_market`), use [hidden events + bindings](../validation/script-telemetry-via-hidden-events.md). Binding matrix: [Script logging and telemetry](../validation/script-logging-and-telemetry.md).

Prefix infra files `SYS-` so contributors know they are not gameplay features.

# Balance spreadsheet export

Parse buildings / PMs / advances → Excel for profitability passes. Keep spreadsheet as the design surface; regenerate game files from templates where possible.

# Autosave storer

Long balance runs overwrite autosaves. Watch the save folder, rename with in-game date, thin by month interval.

# Related

- [CSV location templates](/map/csv-location-templates-pipeline.md)
- [RGO substitution](/buildings/rgo-to-building-substitution.md)
- [Error log debugging](/validation/error-log-debugging.md)

# Citations

[1] MnT `tools/readme.txt`, `tools/shared/`
[2] MnT `tools/BOM_finder/`, `tools/generators_from_game_data/`
[3] MnT `tools/plot/`, `in_game/events/SYS-CENSUS.txt`, `SYS-scripted_effect.txt`
[4] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
