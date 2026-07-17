---
type: Playbook
title: Script telemetry via hidden events
description: >-
  Copy-paste recipe for structured error_log lines from EU5 script when bare
  scripted_effects have no scope. Includes binding fixes from REQ-009 sessions.
tags: [validation, debugging, error_log, events, telemetry]
timestamp: 2026-07-16T08:55:00+10:00
status: complete
source_mod: "Sire, Who Bound This Manor to a Single Merchandise"
---

**Concept map:** [Script logging and telemetry](script-logging-and-telemetry.md) — binding matrix, vanilla survey, script vs UI prices.

# Problem

`error_log` from a **scripted_effect** (pulse → location pick) leaves bindings empty except globals. Even inside a hidden `location_event`, **`ROOT.GetOwner` / `ROOT.GetKey` often fail**.

Use **saved scopes** + **owner stashed variables** for script numerics.

# Recipe — location-scoped AI pick

## 1. Save scopes in the real effect

```txt
rgo_conv_ai_pick_and_convert = {
	save_scope_as = rgo_conv_ai_pick_loc
	ordered_goods = {
		order_by = rgo_conv_ai_pick_order_score
		limit = { rgo_conv_ai_ordered_candidate_ok = yes }
		max = 1
		save_scope_as = rgo_conv_ai_pick_good
	}
	if = {
		limit = { exists = scope:rgo_conv_ai_pick_good }
		rgo_conv_ai_log_pick = yes
		rgo_conv_ai_start_picked_good = yes
	}
}
```

## 2. Stash script prices on location scope + fire event

```txt
rgo_conv_ai_log_pick = {
	set_variable = { name = rgo_conv_dbg_p_from  value = rgo_conv_ai_dbg_p_from  days = 1 }
	set_variable = { name = rgo_conv_dbg_p_to    value = rgo_conv_ai_dbg_p_to    days = 1 }
	set_variable = { name = rgo_conv_dbg_p_floor value = rgo_conv_ai_dbg_p_floor days = 1 }
	trigger_event_silently = rgo_conversion.901
}
```

Do **not** put `p_from` / `p_floor` inside `owner = { }` — `raw_material` script_values evaluate to 0 there.

Script values (`in_game/common/script_values/rgo_conv_ai_debug_values.txt`) — **same `price_in_market()` as AI**:

```txt
rgo_conv_ai_dbg_p_from = {
	scope:rgo_conv_ai_pick_loc.raw_material = {
		value = "price_in_market(scope:rgo_conv_ai_pick_loc.market)"
	}
}
rgo_conv_ai_dbg_p_to = {
	scope:rgo_conv_ai_pick_good = {
		value = "price_in_market(scope:rgo_conv_ai_pick_loc.market)"
	}
}
rgo_conv_ai_dbg_p_floor = { value = rgo_conv_ai_min_margin_price }
```

Use **dot notation** `scope:rgo_conv_ai_pick_loc.raw_material` — nested `scope:rgo_conv_ai_pick_loc = { raw_material = { … } }` may stash as 0.

## 3. Hidden location_event

```txt
namespace = rgo_conversion

rgo_conversion.901 = {
	type = location_event
	hidden = yes
	title = empty_text
	desc = empty_text
	outcome = neutral

	immediate = {
		save_scope_as = rgo_conv_ai_pick_loc
		error_log = rgo_conv_ai_pick_log
	}
}
```

## 4. Localization (UTF-8 BOM; mirror under `main_menu/localization/`)

```yml
rgo_conv_ai_pick_log: "SIRE_AI_PICK … p_from=[SCOPE.sLocation('rgo_conv_ai_pick_loc').MakeScope.GetVariable('rgo_conv_dbg_p_from').GetValue|2] p_to=[SCOPE.sLocation('rgo_conv_ai_pick_loc').MakeScope.GetVariable('rgo_conv_dbg_p_to').GetValue|2] p_floor=[SCOPE.sLocation('rgo_conv_ai_pick_loc').MakeScope.GetVariable('rgo_conv_dbg_p_floor').GetValue|2] ui_from=[SCOPE.sLocation('rgo_conv_ai_pick_loc').GetMarket.GetPrice(SCOPE.sLocation('rgo_conv_ai_pick_loc').GetRawMaterial)|2] ui_to=[…]"
```

| Field prefix | Source | AI uses? |
|--------------|--------|----------|
| `p_*` | `price_in_market()` via script_values | **Yes** |
| `ui_*` | `GetMarket.GetPrice()` | No — compare only |

Every pick should satisfy `p_to >= p_floor`. If `ui_*` fails margin but `p_*` passes, APIs diverge — not an AI bug.

**Validated session (Sire REQ-009, 2026-07-16):** 135 picks after full margin fix — **135/135** `p_to >= p_floor` and `ui_to >= 1.5×ui_from`; 0 sand/clay targets. Pre-fix baseline: 457 picks, 3.7% margin pass, 56% sand/clay.

# Binding cheatsheet

| Need | Use | Do not use |
|------|-----|------------|
| Tag, loc id, from good | `SCOPE.sLocation('saved').…` | `ROOT.GetOwner` in loc |
| To good | `SCOPE.sGoods('saved').GetKey` | — |
| Script numeric | `SCOPE.sLocation('saved').MakeScope.GetVariable(…)` | `owner` block for `raw_material` values; `ROOT.MakeScope` on location |
| UI market price | `SCOPE.sLocation(…).GetMarket.GetPrice(…)` | As sole source for AI validation |

# Log output

`Documents/Paradox Interactive/Europa Universalis V/logs/error.log`:

```text
[08:52:18][jomini_effect_impl.cpp:495]: events/rgo_conv_ai_debug_events.txt:14: SIRE_AI_PICK year=1339 tag=CND loc=6851 from=clay to=legumes p_from=0.29 p_to=0.77 p_floor=0.43 ui_from=0.29 ui_to=0.77
```

```powershell
Select-String -Path "$env:USERPROFILE\Documents\Paradox Interactive\Europa Universalis V\logs\error.log" -Pattern "SIRE_AI_PICK"
```

# Country-scoped telemetry

`type = country_event`; fire with `owner = { trigger_event_silently = … }`. In country events, `ROOT.GetVariable('x').GetValue` may work without `SCOPE` (vanilla tooltip pattern). Still prefer `save_scope_as` + `SCOPE.sCountry` for consistency.

Vanilla hidden AI events (no logging): `game/in_game/events/ai_area_conqest_events/hidden_events_for_ai_conquest.txt`.

# Ship discipline

* Separate debug files + manifest checklist — **Sire archive:** `development/Sire…/references/telemetry-archive/REQ-009-ai-pick/`
* Remove `rgo_conv_ai_log_pick = yes` hook from shipping effects  
* **Fully quit EU5** after editing telemetry — new game does not reload script  

# Analytics pipeline (Sire REQ-009)

After enabling telemetry and running AI On:

```powershell
cd development/Sire, Who Bound This Manor to a Single Merchandise
python scripts/export_sire_ai_pick_csv.py      # → 07_Test/data/sire_ai_picks_latest.csv
python scripts/analyze_sire_ai_picks.py        # → 07_Test/results/…-telemetry-analysis.md
python scripts/classify_ai_pick_realism.py     # optional realism flags
```

Product OKF: `references/REQ-009-ai-pick-telemetry-analytics.md` (rotated logs, last-game filter, CSV schema, interpretation).

# Alternatives

| Approach | Use when |
|----------|----------|
| Static `error_log = "…"` | Unreachable branch only (vanilla style) |
| MnT `::TG::` delimiter + macros | Yearly balance dumps — [toolchain](/tooling/total-conversion-toolchain.md) |

# See also

* [Script logging and telemetry](script-logging-and-telemetry.md)
* [Error log debugging](error-log-debugging.md)

# Citations

[1] Sire REQ-009 — full binding matrix  
[2] `docs/effects.log` — `error_log`, `set_variable`  
[3] Vanilla `flavor_chi_treasure_expedition.txt` — static assert only
