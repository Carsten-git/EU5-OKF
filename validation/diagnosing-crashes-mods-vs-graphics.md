---
type: Playbook
title: Diagnosing crashes — mods vs graphics
description: How to tell script/mod crashes from Vulkan ErrorDeviceLost frontend GPU failures using crash folders.
tags: [validation, crash, vulkan, graphics, debugging]
timestamp: 2026-07-12T10:00:00+10:00
status: complete
source_mod: rgo_conversion
---

# Crash folder layout

Hard crashes land under:

`Documents/Paradox Interactive/Europa Universalis V/crashes/<AppName><timestamp>/`

| File | Use |
|------|-----|
| `exception.txt` | Exception code + stack |
| `meta.yml` | Build, GPU, **RenderAPI**, mods list, **Idler** (Frontend vs InGame) |
| `logs/error.log` | Last engine errors before death |
| `logs/debug.log` | Mount order, mod enable lines |

# Frontend Vulkan device lost (not a script bug)

**Signature (observed 2026-07-12, EU5 1.3.10, AMD RX 6600M, Vulkan):**

* `exception.txt`: `EXCEPTION_ACCESS_VIOLATION`
* `error.log`: `Gfx error: _Queue.submit(...) returned: ErrorDeviceLost`
* `meta.yml` → `"Idler 1": FrontendInterfaceIdler` → crash in **main menu**, not map
* Stack often mentions graphics / NGX shutdown symbols
* No `jomini_script_system` / `rgo_conv` / PostValidate spam tied to the crash moment

**Implication:** Do **not** blame gameplay mods first. Same crash can recur after the launcher disables mods (sentinel: “shut down unexpectedly before InGame”). Observed both with Mod_1 listed and with mods stripped.

**Also graphics (even if Idlers include InGame):** stacks with `ffxFsr2ResourceIsNull` / `NVSDK_NGX_*` and no `ErrorDeviceLost` line can still be GPU/upscaler failures mid-session — not script. Reaching `InGameInterfaceIdler` only proves the map loaded; check the stack before blaming the mod.

**Mitigations to try:** switch RenderAPI away from Vulkan (e.g. DirectX), lower/disable FSR/upscaling, update/rollback GPU driver, free VRAM/RAM, avoid stacking `--debugmode` / GUI validation if unstable.

# When it *is* likely the mod

* Stack is gameplay/script (not Vulkan queue / FSR / NGX), **and**
* `error.log` shows script PostValidate, unknown effects, infinite loops, bad iterators (`every_location` — [KI-061](known-issues.md)) near the crash, **and**
* Crash reproduces only with the mod enabled and a specific scripted action

# Sentinel / auto-disable mods

```
Found sentinel file from a previous boot, game was shut down unexpectedly before InGame stage.
… disabling mods this startup …
```

Means the **previous** boot died before reaching the map. Next boot may strip mods so the in-game mod manager still works — not proof the mod caused the GPU crash.

# Harmless metadata noise

Missing `.metadata/metadata.json` for folders under `mod/` that are knowledge bases or incomplete mods → `Mod metadata read error` only. Not a crash cause.

# See also

* [Error log debugging](error-log-debugging.md)
* [Common pitfalls](common-pitfalls.md)
* [KI-072](known-issues.md)

# Citations

[1] Crash dumps `Europa Universalis V20260711_235003` / `_235032` / `20260712_013408` — `ErrorDeviceLost` + `FrontendInterfaceIdler` with and without mods
[2] `meta.yml` Mod_1 vs disabled-mod relaunch still crashing the same way
[3] In-session dumps with `InGameInterfaceIdler` but stacks dominated by `ffxFsr2ResourceIsNull` / NGX — still graphics
