---
type: Playbook
title: Error log debugging
description: Reading EU5 logs under Documents/Paradox Interactive to diagnose mod load and script errors.
resource: Documents/Paradox Interactive/Europa Universalis V/logs/
tags: [validation, debugging, logs]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

When a mod fails silently in-game, the first place to look is the log folder under your Paradox user directory — not the Steam install.

# Log location (Windows)

```
Documents/Paradox Interactive/Europa Universalis V/logs/
├── error.log          ← primary mod debugging file
├── game.log           ← gameplay flow, less script detail
├── debug.log          ← verbose engine output
├── setup.log          ← load / parse phase
├── gui.log            ← UI issues
└── error.1.log …      ← rotated previous sessions
```

Path on this machine:

`C:\Users\Carst\Documents\Paradox Interactive\Europa Universalis V\logs\`

**Workflow:** reproduce the issue in-game → quit to desktop (flushes logs) → open `error.log` → search from the **bottom** for your session timestamp.

# What to search for

| Pattern | Meaning | Mod action |
|---------|---------|------------|
| `jomini_script_system.cpp` + `Script system error` | Invalid effect/trigger at named file:line | Fix script syntax or scope |
| `pdx_mod_metadata.cpp` + `Mod metadata read error` | Missing or corrupt `.metadata/metadata.json` | Add/fix [mod metadata](/getting-started/mod-metadata-and-descriptor.md) |
| `Could not create … due to 'fopen() failed'` | File encoding or missing file | Check UTF-8 BOM on yml; verify path |
| `Tried to localize … Key: "foo"` | Missing localization key | Add key to yml; confirm BOM and `*_l_english.yml` suffix |
| `unknown modifier` / invalid key (when present) | Bad stat key | Cross-check [modifier stat keys](/modifiers/modifier-stat-keys.md) |
| `diplomatic_action_scripted.cpp` | Scripted interaction missing `category` | Add category field to interaction def |
| `Could not find promote` / `Failed converting statement` | `error_log` loc binding invalid for scope | See [Script logging and telemetry](script-logging-and-telemetry.md) binding matrix |
| `rgo_conv_ai_pick_log` (or other `*_log` key) with **no** `SIRE_*` prefix | Telemetry loc yml missing UTF-8 BOM — bindings not loaded | [KI-078](known-issues.md), [UTF-8 BOM](/localization/utf8-bom-requirement.md) |

Many mod mistakes (**wrong modifier key on advances**, **event never fires**) produce **no** log line. Combine log reading with [smoke testing](smoke-testing-checklist.md) and [validate_mod.py](mod-validation-tooling.md).

# Scoped telemetry (`error_log` from script)

Bare `error_log` in **scripted_effects** has **no usable ROOT/SCOPE** — only globals like `GetCurrentYear` resolve. For structured picks (tag, loc, goods, script prices), use a **hidden event** + saved scopes + owner `set_variable`. Full reference: [Script logging and telemetry](script-logging-and-telemetry.md). Copy-paste recipe: [Script telemetry via hidden events](script-telemetry-via-hidden-events.md).

| Log noise | Meaning |
|-----------|---------|
| `Promote 'ROOT' returned nullptr` | Used `ROOT.*` in loc — switch to `SCOPE.sLocation('saved')` |
| `Could not find promote for 'MakeScope'` | `ROOT.MakeScope` on location — stash on owner + `SCOPE.sCountry(…).MakeScope` |
| `Could not find promote for 'GetVariable'` | `GetOwner.GetVariable` chain — use saved country scope |
| `Failed converting statement for '…'` | Binding omitted from printed line — field will be missing |
| `Variable '…' is set but is never used` … `localization doesn't count` | Expected for telemetry `set_variable` |

After editing telemetry files, **fully quit EU5** before testing script changes; a new campaign does not reload script. **Loc/BOM fixes may hot-reload** in-session — verify with `Select-String … SIRE_AI_PICK` ([KI-078](known-issues.md)).

# Filtering tips

PowerShell — last 50 script errors:

```powershell
Select-String -Path "$env:USERPROFILE\Documents\Paradox Interactive\Europa Universalis V\logs\error.log" -Pattern "Script system error" | Select-Object -Last 50
```

Search for your mod prefix:

```powershell
Select-String -Path "…\logs\error.log" -Pattern "teu_nc_"
```

Ripgrep (from any directory):

```bash
rg "teu_nc_|Script system error" "$USERPROFILE/Documents/Paradox Interactive/Europa Universalis V/logs/error.log"
```

# Crash dumps

Hard crashes copy logs to:

`Documents/Paradox Interactive/Europa Universalis V/crashes/<timestamp>/`

Always read **`exception.txt`**, **`meta.yml`** (Idler, RenderAPI, Mod_*), and **`logs/error.log`** together.

| Signal | Likely class |
|--------|----------------|
| `ErrorDeviceLost` + `FrontendInterfaceIdler` | Graphics/Vulkan — [mods vs graphics](diagnosing-crashes-mods-vs-graphics.md) |
| Script PostValidate / unknown effect + InGame | Mod script |
| Sentinel “disabling mods this startup” | Previous boot died **before** InGame — next launch may strip mods |

Compare crash `error.log` with the main logs folder — the last lines before exit often show the failing system.

# Debug launch (optional)

Running with `-debug_mode` (launcher or Steam launch options) increases log verbosity. Useful for localization asserts and load-order issues; noisy for routine play.

# Ignore noise

Vanilla sessions log many benign lines — achievement stubs, audio Wwise missing events, ruler term data warnings. Focus on lines that cite **your mod path** or **your scripted file names**.

# See also

* [Script logging and telemetry](script-logging-and-telemetry.md)
* [Script telemetry via hidden events](script-telemetry-via-hidden-events.md)
* [Mod validation tooling](mod-validation-tooling.md)
* [Common pitfalls](common-pitfalls.md)
* [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)

# Citations

[1] `Documents/Paradox Interactive/Europa Universalis V/logs/error.log` — live session samples
[2] [Modding wiki — error.log and encoding](https://eu5.paradoxwikis.com/Modding)
