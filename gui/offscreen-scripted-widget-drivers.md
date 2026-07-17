---
type: Playbook
title: Off-screen scripted widget drivers
description: Register always-on off-screen scripted widgets to run GUI-only bindings (construct queues, classification, map-mode sync) outside player-visible panels.
tags: [gui, scripted-widgets, construction, cmf, workaround]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

Some EU5 operations and data bindings are only reliable from **GUI scope** (construct cost / can-build, certain building queries). Construction Manager (CM) pattern **CM-1**: keep tiny **scripted widgets** loaded forever, parked off-screen, and drive them with state / animation hooks.

# Problem

Monthly script pulses can **stage** candidates cheaply, but the actual construct / re-check of engine GUI bindings must happen where those bindings exist. A player-visible window is the wrong place for background automation.

# Registry

`in_game/gui/scripted_widgets/<mod>_scripted_widgets.txt`:

```txt
gui/cm_hidden_window.gui = cm_hidden_window
gui/cm_construct_queue_window.gui = cm_construct_queue_window
gui/cm_food_map_mode_window.gui = cm_food_map_mode_window
```

Engine loads these at session start (same mechanism as Glorp settings windows).

# Off-screen always-visible shell

```txt
widget = {
	name = "cm_hidden_window"
	visible = "[EqualTo_CFixedPoint('(CFixedPoint)0', '(CFixedPoint)0')]"  # always true
	position = { -10000 1 }
	# nested widgets + states call scripted_gui / PdxGuiTriggerAllAnimations
}
```

| Technique | Purpose |
|-----------|---------|
| Far-negative `position` | Not visible to player |
| Always-true `visible` | Widget stays in the tree so states can fire |
| Nested `state` / `on_finish` | Chain into scripted_gui classification or queue drain |
| Gate with `GetPlayer.Exists` + scripted_gui `IsShown` | Avoid work before country ready |

# Split: stage in script, drain in GUI

**Monthly / CMF pulse (script):**

1. Clear / fill country variable lists of candidate locations + building/RGO types.
2. Set a flag such as `cm_should_construct` when anything staged.
3. Do **not** rely on script alone for final construct if GUI bindings are required.

**Construct queue window (GUI):**

1. While flag set, fire scripted_gui executors (`cm_gui_try_rgo`, etc.).
2. Re-validate gold / profit gates at drain time.
3. Clear cycle state when empty or stalled (CM resets after skipped pulses).

Reusable lesson: **pulse = planner; scripted widget = worker**.

# Sibling uses in CM

| Widget | Job |
|--------|-----|
| `cm_hidden_window` | One-shot / iterative building-type classification into maps (GUI-only queries) |
| `cm_construct_queue_window` | Drain staged constructs |
| `cm_food_map_mode_window` | When `IsLateralViewOpened('food_production')`, call `SetMapMode` |

# Not this pattern

- `cm_open_settings_window.gui` is a **template** (`using = cm_rightclick_open_settings_window`), not a scripted widget — opens CMM via right-click.
- Glorp dual-settings fallback windows are player-facing toggles; see [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md).

# Risks

- Always-on widgets cost CPU if states fire every frame — gate tightly.
- Stall detection needed if drain never clears the flag (CM tracks skipped pulses).
- Confidence: **high** for the architecture; exact binding names are CM-specific.

# Related

* [Construction Manager as reference](/references/construction-manager-as-reference.md)
* [Mass-action UI counter](/gui/mass-action-ui-counter.md) — different (visible batch UI)
* [CMM priority feature dispatcher](/community-mod-framework/cmm-priority-feature-dispatcher.md)

# Citations

[1] CM `in_game/gui/scripted_widgets/cm_scripted_widgets.txt`
[2] CM `in_game/gui/cm_hidden_window.gui`, `cm_construct_queue_window.gui`
[3] CM `in_game/common/on_action/cm_on_action.txt` — staging + `cm_should_construct`
