---
type: Reference
title: Country pulses
description: monthly_country_pulse and yearly_country_pulse for recurring mod logic.
resource: mod/northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt
tags: [on-actions, pulses]
timestamp: 2026-07-06T09:00:00+10:00
status: complete
---

# Monthly pulse

```txt
monthly_country_pulse = {
	on_actions = { teu_nc_monthly_country_pulse_action }
}

teu_nc_monthly_country_pulse_action = {
	trigger = { tag = TEU … }
	effect = {
		teu_nc_monthly_purpose_pulse = yes
		teu_nc_try_purpose_intro = yes
	}
}
```

Runs **per country per month** when `trigger` passes — ideal for meters, peace counters, and intro-event fallback.

# Yearly pulse

Use for expensive refresh or save-game bootstrap:

```txt
yearly_country_pulse = {
	on_actions = { teu_nc_yearly_purpose_refresh }
}
```

# Design tips

- Reset "this month delta" variable at start of pulse, then accumulate changes.
- Re-apply display modifiers each month so tier changes stay visible.
- Gate with `tag =` or `has_variable =` to avoid running on every country in the world.
- Keep handlers thin: `monthly_country_pulse` runs for **every country every month**. Heavy `random_owned_location` / allow-matrix limits belong behind rare chance or yearly cadence — [Mod performance](/on-actions/mod-performance-pulses-and-scans.md).

# See also

* [On-actions overview](on-actions-overview.md)
* [Variables and monthly mechanics](/scripted-effects/variables-and-monthly-mechanics.md)
* [Mod performance — pulses and location scans](/on-actions/mod-performance-pulses-and-scans.md)

# Citations

[1] [Northern Crusade on_action hooks](mod/northern_crusade_teu/in_game/common/on_action/teu_nc_purpose.txt)
[2] [Vanilla on_action hardcoded pulses](game/in_game/common/on_action/_hardcoded.txt)
[3] [On-actions overview](on-actions-overview.md)
