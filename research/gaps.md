# Cursor AI Mastery — Gap Tracking

Last reviewed: 2026-03-23

## Open

- [ ] **Custom modes config** — `.cursor/modes.json` file is under consideration but not yet shipped; monitor for release (2026-03-22)
- [ ] **Automations deep dive** — New Automations feature (Mar 5, 2026) needs detailed guide with trigger types, configuration, and examples (2026-03-23)
- [ ] **Composer 2 model details** — Cursor's own Composer 2 model launched Mar 19; need pricing breakdown and performance benchmarks (2026-03-23)
- [ ] **MCP Apps guide** — Interactive UIs in agent chats (Cursor 2.6) need documentation with examples (2026-03-23)
- [ ] **Team marketplace setup** — Private plugin marketplaces for Teams/Enterprise need configuration guide (2026-03-23)
- [ ] **Pricing guide update** — Credit-based pricing (5 tiers: Hobby/Pro/Pro+/Ultra/Teams) needs detailed breakdown with token costs per model (2026-03-23)

## Resolved

- [x] **Marketplace plugin details** — Verified install commands and descriptions for Render, Clerk, JFrog, Miro, Meta Reality Labs, Plain, Circle, Phantom Connect (2026-03-23)
- [x] **cursor.directory deep dive** — Updated with live community data: 75.1K+ devs, 130+ plugins, trending board (2026-03-23)
- [x] **Hooks configuration format** — Confirmed: `.cursor/hooks.json` with `version: 1`, 6 lifecycle events (beforeSubmitPrompt, beforeShellExecution, beforeMCPExecution, beforeReadFile, afterFileEdit, stop) (2026-03-23)
- [x] **MCP Hub wrapper** — Verified: `mcp-hub-mcp` by warpdev/tpavelek is real, also `mcp-hub` by ravitemer and `@naotaka/mcp-proxy-hub` (2026-03-23)
- [x] **Cloud agent environment.json** — Verified fields: baseImage, install, start, terminals (name/command/ports), env, persistedDirectories (2026-03-23)
