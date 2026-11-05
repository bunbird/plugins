# Bunbird plugins

Distribution package for [Bunbird](https://bunbird.com): per-client plugins that connect an AI agent to Bunbird's hosted MCP connector, plus one skill that tells the agent how to use it. There is no server code here.

| Connector | URL | Auth | For |
|---|---|---|---|
| `bunbird` | https://mcp.bunbird.com/mcp | OAuth, at connect | everything: the guide, drafts, previews, publishing, changing and sharing apps |

There is one connector. The client starts the sign-in when it connects, and every tool works for the person who signed in.

## Status

The MCP is live at `mcp.bunbird.com`. Access is by invitation: anyone can install the plugin, but signing in needs an invited Bunbird account.

## Install

Claude Code:

```
/plugin marketplace add bunbird/plugins
/plugin install bunbird@bunbird
```

Codex:

```
codex plugin marketplace add bunbird/plugins
```

Then open Codex, run `/plugins` and install Bunbird.

Gemini CLI:

```
gemini extensions install https://github.com/bunbird/plugins
```

Cursor: press **Add Bunbird to Cursor** at https://app.bunbird.com/connect for the connector, or copy `plugins/cursor/bunbird` to `~/.cursor/plugins/local/bunbird` for the connector, skill and rule. Then press **Authenticate** in **Customize**.

**Accept the connector after install.** A plugin carries the MCP server, but the client asks the person to accept it and to sign in. Until that is done the skill loads with no tools behind it. Per-client steps are in each plugin's README under `plugins/`.

## Layout

- `canonical/` is the only hand-edited source: `facts.json` (names, URLs, the connector), `skill.md` (the skill body), `platform-notes/<client>.md` (client-specific extras).
- `scripts/build.mjs` generates everything else: `plugins/`, the three marketplace catalogs, `gemini-extension.json`, `GEMINI.md`, `AGENTS.md`, root `SKILL.md`, and `.gitattributes`.

## Editing

1. Edit files under `canonical/` only. Never edit a generated file.
2. `node scripts/build.mjs`
3. `node scripts/build.mjs --check` (fails on any drift, or any missing or extra file in `plugins/`)

`npm run build` and `npm run check` do the same. Node 20 or newer; the generator has no dependencies.

Run `bun install` once after cloning: it installs lefthook, whose pre-commit hook runs the drift check on every commit. On GitHub, `Plugins: Drift` runs the same check on every pull request and push to `main`, and `CI: Actionlint` and `CI: Zizmor` check the workflows when `.github/` changes.

## Versions

Claude Code plugin manifests and marketplace entries carry no `version`. A version pins the plugin and commits then ship nothing to existing installs; without one the commit is the version. Codex and Gemini want a version, which comes from `canonical/facts.json`.

The skill names the tools, so any MCP tool rename in platform must update this repo in the same batch. Clients cache the tool list at install, so add tools server-side rather than renaming.

## Contributing and security

Changes go through a pull request with a review. Questions and support: support@bunbird.com. Report vulnerabilities privately, as [SECURITY.md](SECURITY.md) says.

## License

[MIT](LICENSE).
