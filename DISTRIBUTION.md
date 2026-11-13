# Distribution

One source (`canonical/`), several client packages, built by `scripts/build.mjs`.

| Client | Package | Catalog | Notes |
|---|---|---|---|
| Claude Code | `plugins/claude-code/bunbird` | `.claude-plugin/marketplace.json` | no `version` in manifest or catalog |
| Codex | `plugins/codex/bunbird` | `.agents/plugins/marketplace.json` | `.codex-plugin/plugin.json`, versioned |
| Cursor | `plugins/cursor/bunbird` | `.cursor-plugin/marketplace.json` | rule `.mdc` plus skill |
| Gemini CLI | repo root | `gemini-extension.json` | context file `GEMINI.md`, `httpUrl` transport |
| OpenAI directory (ChatGPT/Codex) | `plugins/openai/bunbird` | none (uploaded as a ZIP) | Agent Plugins format: `plugin.json` + `mcp.json`, see below |
| Antigravity | `plugins/antigravity/bunbird` | none (Marketplace form) | `plugin.json`, `mcp_config.json` with `serverUrl`, skill, logo, versioned |
| Others | repo root | none | `SKILL.md`, `AGENTS.md` |

Shipping a change: edit `canonical/`, run `node scripts/build.mjs` and `node scripts/build.mjs --check`, commit. Claude Code installs pick up the commit on refresh. Bump `version` in `canonical/facts.json` for Codex, Gemini and Antigravity.

Agent docs served by the platform: `llms.txt`, `llms-full.txt` and `.well-known/agent-skills/index.json` on `app.bunbird.com`. The guide itself is read through the MCP `read_guide` tool.

## OpenAI directory package

`plugins/openai/bunbird` is the upload for the OpenAI plugin dashboard (Plugins, "Upload new or existing plugin", "With MCP"; Linear BUN-420). It uses the Agent Plugins format: root `plugin.json` (listing fields under `extensions.com.openai.interface`, plus `review` and `publication`) and `mcp.json` with exactly one remote server (`streamable-http`, OAuth with `dcr`, no `headers`, no secrets). The onboarding skill is `skills/get-started/SKILL.md`. Docs: https://developers.openai.com/plugins/deploy/submission and https://developers.openai.com/plugins/build/plugins.

Make the ZIP (the generator has no dependencies, so it writes the folder and you zip it; the manifest must sit at the ZIP root):

```
node scripts/build.mjs
cd plugins/openai/bunbird && zip -r ../bunbird-openai.zip . -x '.DS_Store'
```

Limits are asserted by the generator: `displayName` and `shortDescription` 30 characters, `longDescription` 4000, `developerName` 80, `defaultPrompt` 3 prompts of 128. Because of the 30-character limit, `product.shortDescription` is "Build and publish web apps" for every client.

Review content lives in `canonical/openai-review.json`: exactly 5 positive and 3 negative `test_cases` plus `demo_recording_url`. While `test_cases` is empty the manifest omits it (OpenAI then keeps any saved cases); the generator fails if the counts are not 5 and 3. Reviewer credentials never go in the package (the ZIP rejects `test_credentials` and `reviewer_instructions`): enter them in the dashboard Review details. The reviewer account must work without MFA, email or SMS codes or magic links.

Before submitting: verify the domain by serving the challenge token at `https://mcp.bunbird.com/.well-known/openai-apps-challenge`, register OpenAI's OAuth callback with the platform (DCR is used), and confirm `category` against the dashboard's category list.

## Antigravity

`plugins/antigravity/bunbird` is the package. It cannot be the repository root: `agy plugin validate --strict`, which the Marketplace requires, warns that the root's `AGENTS.md` and `GEMINI.md` are never loaded. Subfolder installs from a GitHub URL are not documented, so install from a clone: `agy plugin install plugins/antigravity/bunbird`.

Validate before submitting to the Marketplace: `agy plugin validate --strict plugins/antigravity/bunbird`. Submit through https://forms.gle/2EX5RFYPoJe1UgxR9.

## MCP Registry

`server.json` at the root is the entry for the official MCP Registry (https://registry.modelcontextprotocol.io), which VS Code's server gallery reads. It is generated from `canonical/facts.json` (`registry.name`, `registry.description`, at most 100 characters, and `version`). The name `com.bunbird/bunbird` is proved by the public key served at https://bunbird.com/.well-known/mcp-registry-auth (landing repository); the private key is not in any repository.

A published version cannot change: bump `version` in `canonical/facts.json`, rebuild, then from this directory:

```
mcp-publisher login http --domain bunbird.com --private-key "$(cat <path to private-key.hex>)"
mcp-publisher publish
```

OAuth needs no field: clients find the sign-in from the server itself.

## Icons

Source art is `canonical/assets/` (`icon-512.png` and `icon.svg`, copied unaltered from `bunbird/media` `favicon/icon-512.png` and `favicon/favicon.svg`). Where a client documents a field, the generator copies it into the package and references it:

| Client | Field | File |
|---|---|---|
| Claude Code | `icon` in `.claude-plugin/plugin.json` (read by Anthropic's directory only) | `assets/icon.png` |
| Codex | `interface.logo`, `interface.composerIcon` | `assets/icon.png` |
| Cursor | `logo` in `.cursor-plugin/plugin.json`, path relative to the plugin | `assets/icon.svg` |
| OpenAI package | `interface.logo`, `interface.composerIcon` (square, 48px minimum, PNG/JPEG/WebP/SVG, 5 MiB) | `assets/icon.png` |
| Antigravity | `logo` in `plugin.json` (1:1, 128px minimum, about 10% padding) | `assets/icon.png` |
| Gemini CLI | none: `gemini-extension.json` documents no icon field | |

Docs: https://code.claude.com/docs/en/plugins-reference, https://developers.openai.com/codex/plugins/build, https://cursor.com/docs/plugins/building, https://geminicli.com/docs/extensions/reference/.
