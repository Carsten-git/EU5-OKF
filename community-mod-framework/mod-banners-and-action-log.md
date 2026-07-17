---
type: Playbook
title: CMF mod banners and action log
description: Lobby mod banners and the shared in-game mod action log API.
tags: [cmf, banners, logging, lobby]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

# Mod banners (pre-game lobby)

```txt
cmf_on_banner_registration = {
	on_actions = { your_mod_register_lobby_banner }
}

your_mod_register_lobby_banner = {
	effect = {
		cmf_register_lobby_banner = { mod_id = your_mod }
	}
}
```

Game concepts: `<mod_id>_banner_logo`, `<mod_id>_banner_background`. Loc: `<mod_id>_name`, `<mod_id>_desc`.

# Mod action log

Shared session log (Mod Menu → General → Session → Mod Action Log).

```txt
cmf_log = { action = my_mod_action_key }
cmf_log_with_args = { action = transferred_to arg1 = paris arg2 = FRA }
cmf_log_with_scope_arg = { action = … }      # needs scope:cmf_log_arg2
cmf_log_with_scope_args = { action = … }     # scope:cmf_log_arg1 + arg2
cmf_clear_log = yes   # disabled in multiplayer
```

Actor = current country scope. Arguments are localization keys (or country scopes for scope variants).

# Related

* [Registration hooks](/community-mod-framework/registration-and-on-action-hooks.md)
* [Data-binding macros](/tooling/data-binding-macros.md) — different telemetry style (error_log)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki — Mod Banners, Mod Action Log
