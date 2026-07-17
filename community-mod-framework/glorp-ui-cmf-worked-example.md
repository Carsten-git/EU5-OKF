---
type: Playbook
title: Glorp UI CMF worked example
description: Complete CMF registration trinity — mod registration, lobby banner, and unified cmf_on_callback.
tags: [cmf, registration, banner, callback, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#4**: one on_action file wires the three CMF entry points every CMF UI mod needs.

```txt
cmf_on_mod_registration = {
	on_actions = { glorpui_on_register_cmf_mod }
}
glorpui_on_register_cmf_mod = {
	effect = { glorpui_register_cmf_mod = yes }
}

cmf_on_banner_registration = {
	on_actions = { glorpui_on_banner_registration }
}
glorpui_on_banner_registration = {
	effect = {
		cmf_register_lobby_banner = { mod_id = glorpui }
	}
}

cmf_on_callback = {
	on_actions = { glorpui_on_cmf_callback }
}
glorpui_on_cmf_callback = {
	effect = { glorpui_handle_cmf_callback = yes }
}
```

Banner textures via game concepts (`glorpui_banner_logo`, `glorpui_banner_background`); loc `glorpui_name` / `_desc`.

Callback handler switches on `var:cmf_callback` to re-sync aliases / react to setting changes (see [CMM aliases](/community-mod-framework/cmm-aliases-for-gui.md)).

# Related

* [Registration and on-action hooks](/community-mod-framework/registration-and-on-action-hooks.md)
* [Mod banners](/community-mod-framework/mod-banners-and-action-log.md)

# Citations

[1] Glorp `in_game/common/on_action/glorpui_cmm_on_actions.txt`
[2] Glorp `in_game/common/game_concepts/glorpui_banner.txt`
[3] Glorp `in_game/common/scripted_effects/glorpui_cmm_effects.txt`
