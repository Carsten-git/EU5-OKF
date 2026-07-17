---
type: Reference
title: Naming and subsystem prefixes
description: Prefix conventions that keep large EU5 overhauls navigable (MnT, epbm, SYS, aaa).
tags: [total-conversion, naming, conventions]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Total conversions accumulate hundreds of files. Consistent prefixes are how humans and agents find the right subsystem.

# Recommended prefix table

| Prefix | Role | Examples |
|--------|------|----------|
| `MnT_` / `mnt_` | Branded feature content / variables | `MnT_centers.txt`, `mnt_income_hist_01` |
| Dedicated mechanic prefix | Cross-cutting system with own hooks | `epbm_` (estate-paid building maintenance) |
| `SYS-` | Dev/telemetry infrastructure, not gameplay | `SYS-CENSUS.txt`, `SYS-scripted_effect.txt` |
| `aaa_` / `000_` | GUI template load-order win | `aaa_epbm_expense_tooltip.gui` |
| `REPLACE:` / `INJECT:` | Vanilla patch operators (in-file) | See [override ladder](/total-conversion/three-root-and-override-ladder.md) |

# Subsystem isolation checklist

For each large mechanic (like EPBM):

1. Own file prefix for effects, events, auto_modifiers, loc.
2. Own on_action hooks or a clear section in the pulse file.
3. Save-compat **rebuild version** global (MnT: `@epbm_rebuild_version`) when state shape changes.
4. Document the prefix in the mod changelog / knowledge base.

# File vs key casing

MnT mixes `MnT_`, `mnt_`, `M&T_`, `Mnt_` — workable but noisy. Prefer **one brand prefix** for new mods (`my_mod_` files, `mm_` variables).

# Citations

[1] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
[2] [Estate-paid building maintenance](/economy/estate-paid-building-maintenance.md)
