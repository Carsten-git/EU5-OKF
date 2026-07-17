---
type: Playbook
title: Data-binding macros
description: Define textual macros for bracket binding strings used in error_log telemetry and scripted effects.
tags: [tooling, data-binding, telemetry, macros]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

`in_game/data_binding/` macros are a **textual preprocessor** for `[bracket]` binding strings — not Jomini scopes. Vanilla ships no `data_binding/` folder; this is a mod pattern.

# Macro schema

```txt
macro = {
	description = "This country"
	definition = "TC"
	replace_with = "THIS.GetCountry"
}

macro = {
	description = "This country's estate 'key'"
	definition = "TCEst(key)"
	replace_with = "TC.GetGovernment.GetEstateFromKey(key)"
}
```

| Field | Meaning |
|-------|---------|
| `definition` | Token in `[...]` (may take params) |
| `replace_with` | Expanded binding path; may nest other macros |
| `description` | Docs only |

# Usage in telemetry

```txt
error_log = "::TG::[GetCurrentYear]:[TC.GetTag]:[TCHasInst('feudalism')]:[TCEN.GetGold]:[SLV('loc_dev')]"
```

Gate dumps with global vars; fire yearly from a coordinator country. Parse with external tools ([toolchain](/tooling/total-conversion-toolchain.md)).

# Scope

MnT uses macros in **scripted effects / error_log**, not in `.gui` files (GUI uses native bindings like `[GetCompleteVersionInfoString]`).

# Related

* [Total conversion toolchain](/tooling/total-conversion-toolchain.md)
* [Pulse orchestration](/on-actions/pulse-orchestration.md)

# Citations

[1] MnT `in_game/data_binding/MnT_logger_macros.txt`
[2] MnT `in_game/common/scripted_effects/SYS-scripted_effect.txt`
[3] MnT `in_game/events/SYS-CENSUS.txt`
[4] MnT `tools/plot/log_parser.py`
