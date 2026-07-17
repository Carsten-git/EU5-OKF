---
type: Playbook
title: Smoke testing checklist
description: Minimum in-game checks after changing events, loc, or advances.
tags: [validation, testing]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

# Before launch

- [ ] `python tools/validate_mod.py --all` ([tooling](mod-validation-tooling.md))
- [ ] Scan [error.log](error-log-debugging.md) after quit-to-desktop
- [ ] Regenerate loc with UTF-8 BOM
- [ ] Grep vanilla for the effect name you are using

# New game (human TEU or relevant tag)

- [ ] Intro / onboarding event pops with **prose**, not loc keys
- [ ] Country modifiers list shows custom meter (name + tier)
- [ ] Hover meter: monthly delta and guide text
- [ ] Mod advances **not** pre-researched at day 1
- [ ] Test advances: UI cost matches `25×(1+research_cost)` — [scaling](/advances/research-cost-scaling.md)
- [ ] DHE browser shows mod DHE event(s) with title and historical blurb
- [ ] Menu crashes: check [mods vs graphics](diagnosing-crashes-mods-vs-graphics.md) before rewriting scripts

# Existing save (if you support migration)

- [ ] After one monthly pulse: meter appears
- [ ] Intro re-fires if migration flag bumped

# After loc-only change

- [ ] Full quit to desktop, relaunch, reload save

# See also

* [Common pitfalls](common-pitfalls.md)

# Citations

[1] Mod: `northern_crusade_teu/tools/validate_mod.py` — pre-launch validation
[2] User: `Documents/Paradox Interactive/Europa Universalis V/logs/error.log` — post-session error scan
[3] Mod: `northern_crusade_teu/` — TEU smoke-test reference implementation
