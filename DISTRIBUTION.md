# Distribution

One source (`canonical/`), several client packages, built by `scripts/build.mjs`.

| Client | Package | Catalog | Notes |
|---|---|---|---|
| Claude Code | `plugins/claude-code/bunbird` | `.claude-plugin/marketplace.json` | no `version` in manifest or catalog |
| Codex | `plugins/codex/bunbird` | `.agents/plugins/marketplace.json` | `.codex-plugin/plugin.json`, versioned |
| Cursor | `plugins/cursor/bunbird` | `.cursor-plugin/marketplace.json` | rule `.mdc` plus skill |
| Gemini CLI | repo root | `gemini-extension.json` | context file `GEMINI.md`, `httpUrl` transport |
| Others | repo root | none | `SKILL.md`, `AGENTS.md` |

Shipping a change: edit `canonical/`, run `node scripts/build.mjs` and `node scripts/build.mjs --check`, commit. Claude Code installs pick up the commit on refresh. Bump `version` in `canonical/facts.json` for Codex and Gemini.

Agent docs served by the platform: `llms.txt`, `llms-full.txt` and `.well-known/agent-skills/index.json` on `saas.bunbird.com`. The guide itself is read through the MCP `read_guide` tool.
