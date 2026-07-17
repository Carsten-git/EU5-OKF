---
type: Reference
title: CMF GUI macros and widget overrides
description: Nand/Nor/Xor GUI macros and CMF extracted vanilla top-level widget definitions for safer overrides.
tags: [cmf, gui, macros, overrides]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

# GUI macros

```txt
visible = "[Nand(Foo, Bar)]"   # not both
visible = "[Nor(Foo, Bar)]"    # neither
visible = "[Xor(Foo, Bar)]"    # exactly one
```

# Improved top-level widget overrides

Vanilla top-level widgets (lateralviews, topbar, …) normally require copying the **entire** GUI file including every type/template. CMF ships extracted type/template definitions so your override can contain **only the top-level widget**, reducing merge conflicts with other mods.

Covered files include: `foreign_country_lateralview.gui`, `government_lateralview.gui`, `ingame_topbar.gui`, `location_window.gui`, `outliner_entries.gui`, `single_unit_window.gui`, `technology_lateralview.gui`.

Prefer this over full-file copies when you only need to patch one top-level widget. Still higher maintenance than scripted GUI / CMM when those suffice. See [override ladder](/total-conversion/three-root-and-override-ladder.md) and [custom UI](/gui/custom-ui-patterns.md).

# Related

* [Custom UI patterns](/gui/custom-ui-patterns.md)
* [aaa_ template precedence](/gui/custom-ui-patterns.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki — GUI Macros, Improved Vanilla Top-Level Widget Overrides
[2] CMF wiki pages of the same names
