---
type: Playbook
title: Compatibility with CMF and Glorp UI
description: What a content mod must and must not ship so it coexists with the Community Mod Framework and Glorp UI.
tags: [cmf, glorp, compatibility, gui, load-order, multi-mod]
timestamp: 2026-10-14T16:45:00+11:00
status: complete
source_mod: glorp.ui
source_version: "10.08.26"
---

A mod is **CMF-compatible** when it uses CMF’s registration hooks and does not replace CMF’s files. It is **Glorp-compatible** when Glorp can stay enabled and the mod’s own features still run. Those are different bars. Glorp itself depends on CMF; depending on CMF does not make a location-panel patch survive Glorp.

Checked against installed Workshop copies: CMF `community_mod_framework` **2.4.1** (`3692202776`, game `1.3.*`) and Glorp UI `glorp.ui` **10.08.26** (`3601047146`, `glorpui_last_updated_game_version` `1.3.10`).

# What “compatible” means

| Bar | Pass | Typical fail |
|-----|------|----------------|
| CMF | CMF loads, your mod registers, you did not overwrite `gui/cmf/` or `gui/cmm/` | Missing dependency, or your files replace the framework |
| Glorp | Glorp’s panels stay, and your feature is still reachable | You and Glorp both ship `location_window.gui` (or another file in the collision list) |

Failure is usually **silent**. The later mod’s copy of that GUI path wins the whole file. The game rarely crashes. See [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) for the RGO-button case.

Later in the playset (drawn **below**) overrides the same relative path. CMF and Glorp must load **before** a mod that is allowed to override them. A merge submod that edits Glorp’s file must load **after** Glorp.

# CMF — required

1. **Launcher dependency** in `.metadata/metadata.json` (UTF-8, no BOM):

```json
"relationships": [
  {
    "rel_type": "dependency",
    "id": "community_mod_framework",
    "display_name": "Community Mod Framework",
    "resource_type": "mod",
    "version": "2.*"
  }
]
```

2. **Workshop required item** on your Steam page (id `3692202776`). The metadata relationship does not do this for you.

3. **Playset:** CMF above your mod. Do not ship any of CMF’s paths. Replacing them replaces the framework for every mod below you:

- `in_game/gui/cmf/`
- `in_game/gui/cmm/`
- `in_game/gui/vanilla/cmf_*`
- `in_game/gui/scripted_widgets/cmf_scripted_widgets.txt` and `cmm_scripted_widgets.txt`

4. **Register on the shared hook.** Add a leaf on_action. Do not paste CMF’s effect body into your file.

```txt
cmf_on_mod_registration = {
	on_actions = { your_mod_on_register }
}
your_mod_on_register = {
	effect = { your_mod_register = yes }
}
```

Inside that effect, call the features you use (`cmm_register_*`, `cmf_add_action_bar_element`, alerts). Details: [Registration and on-action hooks](/community-mod-framework/registration-and-on-action-hooks.md).

5. **Detection id is the id you pass to CMF, not the launcher id.** `cmm_register_*` calls `cmf_register_mod` for you. If you do not use CMM, call `cmf_register_mod = { mod_id = your_id }` yourself or other mods cannot see you. Check with:

```txt
cmf_is_mod_active = { mod_id = your_id }
```

That looks up `flag:your_id` in the global list `cmf_active_mod_ids`. Glorp’s launcher id is `glorp.ui`. Its CMF id is **`glorpui`**.

# CMF — not required

| Optional | When |
|----------|------|
| [Dependency check popup](/community-mod-framework/dependency-check-popup.md) | You want the main menu to offer enabling CMF |
| Lobby banner (`cmf_on_banner_registration`) | You want a pre-game banner |
| `cmf_on_callback` | Action-bar clicks or CMM setting changes |
| `cmf_suppress` | Dead `always = no` branch so unused variables do not warn |
| Human pulses | Monthly/yearly work that should skip AI countries |

Gameplay that never touches CMF UI can stay CMF-free. Campaign-start rules still belong in [game rules](/game-rules/game-rules-vs-cmm.md), not CMM.

# Glorp — required

Glorp replaces vanilla GUI files outright. It does not merge a second mod’s copy. **Do not ship the same filename** if both UIs must be present.

Installed Glorp `10.08.26` owns these `in_game/gui/` files (same names as vanilla):

| File |
|------|
| `battle_lateralview.gui` |
| `country_dhe_lateralview.gui` |
| `economy_lateralview.gui` |
| `expand_raw_goods_lateralview.gui` |
| `foreign_country_lateralview.gui` |
| `goods_production_lateralview.gui` |
| `government_lateralview.gui` |
| `ingame_topbar.gui` |
| `location_window.gui` |
| `map_markers.gui` |
| `multiplayer_chat.gui` |
| `outliner_entries.gui` |
| `production_lateralview.gui` |
| `recruit_location_lateralview.gui` |
| `single_unit_window.gui` |

Glorp-only files (`glorp.gui`, `glorpUI_*.gui`) do not collide with a content mod that uses its own names.

CMF’s extracted type files (`in_game/gui/vanilla/cmf_location_window_vanilla_types.gui` and siblings) use **different paths**, so Glorp does not delete them. They also do not protect your `location_window.gui`. A top-level-only override still loses to Glorp’s full file. See [CMF GUI macros and widget overrides](/community-mod-framework/gui-macros-and-widget-overrides.md).

Events, triggers, effects, modifiers, and on_actions are unaffected. Glorp is a UI mod.

# Glorp — how to add UI anyway

Pick one:

| Approach | Do this | Cost |
|----------|---------|------|
| **No shared GUI file** | Action bar, alert, CMM, event, or a uniquely named `.gui` | Best default. Works with Glorp and the next UI overhaul |
| **Merge submod** | Copy Glorp’s file, add a minimal widget, load **after** Glorp, pin `glorpui_last_updated_game_version` | In-panel control. Rebuild on every Glorp UI drop |
| **Peer widget** | Only if Glorp already instantiates your type | Glorp currently embeds Construction Manager widgets (`cm_auto_expand_rgo_widget_location_window`, `cm_auto_food_widget_location_window` in `location_window.gui`). There is no empty slot for an arbitrary mod |

Detecting Glorp does **not** unload a colliding file. Use detection only to hide a duplicate entry or to skip a code path:

```txt
cmf_is_mod_active = { mod_id = glorpui }
```

You do not need Glorp’s other dependencies (Construction Manager `romaimperator.construction_manager`, Autonomous Diplomats `autonomous_diplomats`, SmartTaxes `romaimperator.smart_taxes`) unless you call their scripted GUI.

# Checklist

- [ ] `metadata.json` depends on `community_mod_framework` `2.*` if you call any `cmf_` / `cmm_` effect
- [ ] Workshop page lists CMF as required
- [ ] No files under CMF’s `gui/cmf`, `gui/cmm`, or `gui/vanilla/cmf_*`
- [ ] Registration is a leaf of `cmf_on_mod_registration`
- [ ] CMF id used for detection is the registered `mod_id` (`glorpui`, not `glorp.ui`)
- [ ] None of the Glorp-owned GUI filenames above are in your mod, **or** the only copy ships in a submod that loads after Glorp and is pinned to a Glorp version
- [ ] Feature still reachable with Glorp enabled (action bar, event, or merged widget)
- [ ] Smoke: CMF + Glorp + your mod. Open the panel you touch. Confirm Glorp chrome and your entry. Reverse the Glorp/your-mod order and confirm which file won

# Related

* [Overview and dependency](/community-mod-framework/overview-and-dependency.md)
* [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md)
* [Glorp UI as reference](/references/glorp-ui-as-reference.md)
* [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md)
* [UI override file layering](/gui/ui-override-file-layering.md)

# Citations

[1] Workshop `3450310/3692202776` — CMF 2.4.1 `.metadata/metadata.json`; `in_game/common/scripted_effects/cmf_effects.txt` (`cmf_register_mod`); `in_game/common/scripted_effects/cmm_core_effects.txt` (`_cmm_register_mod` calls `cmf_register_mod`); `in_game/common/scripted_triggers/cmf_core_triggers.txt` (`cmf_is_mod_active`)
[2] CMF README dependency block and wiki `docs/wiki/cmf.wiki` (Basic Setup, Mod Detection, Improved Vanilla Top-Level Widget Overrides)
[3] Workshop `3450310/3601047146` — Glorp UI 10.08.26 `.metadata/metadata.json` (`id` `glorp.ui`, CMF dependency `2.*`); `in_game/common/scripted_effects/glorpui_cmm_effects.txt` (`mod_id = glorpui`); `in_game/gui/` vanilla-named overrides; `location_window.gui` instantiates `cm_auto_expand_rgo_widget_location_window` and `cm_auto_food_widget_location_window`
