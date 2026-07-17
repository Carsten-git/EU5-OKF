---
type: Playbook
title: CMF utility triggers and effects
description: is_host, cmf_is_mod_active, cmf_suppress, unrestricted tools, and deprecated variable_map helpers.
tags: [cmf, triggers, effects, multiplayer, validation]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

# is_host

```txt
if = {
	limit = { is_host = yes }
	# host-only (always yes in singleplayer)
}
```

# Unrestricted tools

`cmf_can_use_unrestricted_tools` — host (or SP) and “Enable Unrestricted Tools” setting on. Gate dangerous features (e.g. unrestricted land transfer). Mark CMM settings with `cmm_set_requires_unrestricted_tools_enabled` after registration.

# Mod detection

```txt
if = {
	limit = { cmf_is_mod_active = { mod_id = some_other_mod } }
	# …
}

# Non-CMM mods: register so others can detect you
cmf_register_mod = { mod_id = your_mod }
```

Mods that register CMM settings are detected automatically.

# cmf_suppress (engine warnings)

Preferred community alternative to orphan never-fire events for “set but never used” / “used but never set”:

```txt
if = {
	limit = { always = no }
	cmf_suppress = { v = my_variable }
	cmf_suppress = { v = my_flag_name }
}
```

Group many suppress calls in one dead `if` so `always = no` is evaluated once. See also [never-trigger-me](/gui/never-trigger-me-workaround.md) for the pre-CMF pattern.

# Deprecated variable_map helpers

`cmf_change_variable_map` / `_local_` / `_global_` — deprecated; vanilla `add_to_variable_map` now overwrites keys directly.

# Related

* [Never-trigger-me workaround](/gui/never-trigger-me-workaround.md)
* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki — Utility sections
[2] CMF wiki: Warning Suppression, Mod Detection, is_host Trigger
