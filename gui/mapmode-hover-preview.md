---
type: Playbook
title: Mapmode hover preview
description: Temporarily push a map mode override while hovering a location-window stat widget.
tags: [gui, map-modes, ux, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#18**:

On a location stat widget, bind map mode context and push override on hover:

```txt
datacontext = "[GetMapMode('proximity')]"
onmousehierarchyenter = "[PdxGuiWidget.PushMapModeOverride('proximity')]"
```

(Pair with leave/clear handlers as needed — verify against current GUI API in vanilla/Glorp.)

Useful for location-focused mods that preview control/proximity/etc. without forcing a permanent mapmode switch.

# Related

* [Custom map modes](/gui/custom-map-modes.md)
* [Live vs cached mapmode metrics](/gui/live-vs-cached-mapmode-metrics.md)

# Citations

[1] Glorp `in_game/gui/location_window.gui` — proximity hover injection
[2] ZMC / CM custom modes — permanent picker modes (different from hover preview)
