---
type: Playbook
title: Never-trigger-me variable workaround
description: Silence EU5 unused/never-set variable analyzer warnings for GUI-only country variables.
tags: [gui, variables, workarounds, validation]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Variables referenced only from GUI localization or chart widgets can trip engine warnings (“never set” / “set but never used”). A durable workaround is an **orphan event that never fires** but touches those variables in script.

# Pattern

```txt
namespace = mnt_never_trigger_me

mnt_never_trigger_me.1 = {
	type = country_event
	orphan = yes
	hidden = yes
	trigger = {
		current_year = 42069   # impossible
	}
	immediate = {
		# touch every GUI-only var so the analyzer sees read+write
		change_variable = { name = mnt_income_hist_01 multiply = 1 }
		# …
	}
}
```

# Rules

- Keep the event **orphan** and **impossible to trigger**.
- Prefer `multiply = 1` (no gameplay side effect) over setting real values.
- When you add a new GUI-only variable, add it here in the same PR.
- Document the credit/workaround in a file comment (MnT cites community origin).

Group multiple calls in one dead `if` so the engine evaluates `always = no` once.

# Prefer CMF when available

If your mod depends on [Community Mod Framework](/community-mod-framework/overview-and-dependency.md), use [`cmf_suppress`](/community-mod-framework/utility-triggers-and-effects.md) instead of maintaining a growing orphan event.

# Related

* [Custom UI patterns](/gui/custom-ui-patterns.md)
* [Displaying hidden mechanics](/modifiers/displaying-hidden-mechanics.md)
* [Known issues](/validation/known-issues.md)

# Citations

[1] MnT `in_game/events/MnT_never_trigger_me.txt`
