---
type: Reference
title: UTF-8 BOM requirement
description: EU5 localization AND many script files should use UTF-8 with BOM; missing BOM causes silent loc failure or lexer warnings.
resource: mod/northern_crusade_teu
tags: [localization, bom, encoding, pitfall, scripts]
timestamp: 2026-07-11T20:00:00+10:00
status: complete
---

# Problem

## Localization

YAML localization files saved as **UTF-8 without BOM** can fail silently: the game loads zero keys, and UI shows raw loc keys like `flavor_teu_nc_purpose.100.title`.

## Telemetry `error_log` (partial failure — KI-078)

Debug telemetry yml (`rgo_conv_*_log` keys used by `error_log = …` in hidden events) can fail **without** breaking in-game UI:

| With BOM | Without BOM |
|----------|-------------|
| `SIRE_AI_PICK year=1340 tag=FRA loc=…` | `rgo_conv_ai_pick_log` (unresolved loc key only) |
| Export scripts parse rows | `export_*` finds **0** `SIRE_*` lines |
| Funnel / pick analytics work | Conversions still run — only logging is broken |

**Common cause:** file created by editor/agent `Write` / save-as UTF-8 (no BOM); file never committed so BOM was never enforced in review.

**Always mirror** debug loc under `in_game/localization/english/` **and** `main_menu/localization/english/` — both need exactly **one** BOM at file start (not zero, not two — see [KI-081](/validation/known-issues.md)).

## Script / data files

`in_game` / `main_menu` script files (`.txt` under `common/`, `events/`, etc.) also prefer **UTF-8 with BOM**. Without it, `error.log` shows:

```text
lexer.cpp: File 'common/.../foo.txt' should be in utf8-bom encoding (will try to use it anyways)
```

The engine often still loads them, but treat BOM as required for mod scripts to avoid subtle parse issues.

**Exception:** `.metadata/metadata.json` should stay **UTF-8 without BOM** (JSON).

# Fix

| File type | Encoding |
|-----------|----------|
| `*_l_*.yml` loc | UTF-8 **with** BOM (`utf-8-sig`) |
| Script `.txt` (common, events, on_action, …) | UTF-8 **with** BOM |
| `.gui` overrides | UTF-8 with BOM is fine (same lexer family) |
| `.metadata/metadata.json` | UTF-8 **without** BOM |

```python
path.write_text(content, encoding="utf-8-sig")  # loc + scripts
path.write_text(content, encoding="utf-8")      # metadata.json only
```

# Verification

- Hex editor: BOM is `EF BB BF` at the start (loc/scripts).
- Loc: in-game titles show prose, not dotted keys.
- Telemetry: after a few months with AI on, `error.log` contains expanded `SIRE_AI_PICK` / `SIRE_MARKET_PRICE` lines — not bare `rgo_conv_ai_pick_log`.
- Scripts: no `should be in utf8-bom encoding` spam for your mod files in `error.log`.

```powershell
# Quick BOM check (first 3 bytes = EF BB BF)
Format-Hex -Path "in_game\localization\english\rgo_conv_debug_l_english.yml" -Count 3

# After play session
Select-String -Path "$env:USERPROFILE\Documents\Paradox Interactive\Europa Universalis V\logs\error.log" -Pattern "SIRE_AI_PICK"
```

# See also

* [Event localization naming](event-localization-naming.md)
* [Common pitfalls](/validation/common-pitfalls.md)
* [Known issues](/validation/known-issues.md) — KI-010, KI-060, KI-078
* [Script logging and telemetry](/validation/script-logging-and-telemetry.md) — `error_log` binding + BOM

# Citations

[1] Observed during `northern_crusade_teu` — loc without BOM failed silently
[2] Observed during `rgo_conversion` — script `.txt` without BOM → lexer warnings in `error.log`
[3] Sire REQ-009 session 2026-07-18 — `rgo_conv_debug_l_english.yml` without BOM → `error.log` key-only telemetry ([KI-078](/validation/known-issues.md))
