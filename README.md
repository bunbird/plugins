# Bunbird plugins

Distribution package for [Bunbird](https://bunbird.com): per-client plugins that connect an AI agent to Bunbird's hosted MCP server, plus one skill that tells the agent when to use it and how to connect. There is no server code here.

| Server | URL | Auth | For |
|---|---|---|---|
| `bunbird` | https://mcp.bunbird.com/mcp | OAuth | everything, for the signed-in person: the guide, drafts in their organisations, previews, publishing |

## Status

The repository is private until launch, and `bunbird.com` is not live yet. Installs will fail to connect until the domain and servers are up.

## Install

Claude Code:

```
/plugin marketplace add bunbird/plugins
/plugin install bunbird@bunbird
```

Codex:

```
codex plugin marketplace add https://github.com/bunbird/plugins.git
```

Gemini CLI:

```
gemini extensions install https://github.com/bunbird/plugins
```

Cursor: add this repository as a plugin source and install `bunbird`.

**Accept the connector after install.** A plugin carries the MCP server, but the client asks the person to accept it and sign in. Until that is done the skill loads with no tools behind it. Per-client steps are in each plugin's README under `plugins/`.

## Layout

- `canonical/` is the only hand-edited source: `facts.json` (names, descriptions, URLs, the server), `skill.md` (the skill body), `platform-notes/<client>.md` (client-specific extras).
- `scripts/build.mjs` generates everything else: `plugins/`, the three marketplace catalogs, `gemini-extension.json`, `GEMINI.md`, `AGENTS.md`, root `SKILL.md`, and `.gitattributes`.

## Editing

1. Edit files under `canonical/` only. Never edit a generated file.
2. `node scripts/build.mjs`
3. `node scripts/build.mjs --check` (fails on any drift, or any missing or extra file in `plugins/`)

`npm run build` and `npm run check` do the same. Node 20 or newer; the generator has no dependencies.

Run `bun install` once after cloning: it installs lefthook, whose pre-commit hook runs the drift check on every commit. The org runs checks locally, so there is no CI workflow.

## Trying it against a local server

`node scripts/build.mjs --dev` (or `npm run dev`) generates the same packages into `.dev/`, ignored by git, with the server at `https://mcp.bunbird.localtest.me:3443/mcp` (the platform's `make dev`). Pass `--server-url=<url>` or set `BUNBIRD_MCP_URL` for another one. Catalogs there are named `bunbird-dev`, so a dev install sits beside a production one. Committed files and the drift check never see this.

Install from the folder, not from GitHub:

```
# Claude Code (Node does not read the keychain: trust the edge's CA)
NODE_EXTRA_CA_CERTS=<platform>/.data/edge/caddy/pki/authorities/local/root.crt claude
/plugin marketplace add ./.dev
/plugin install bunbird@bunbird-dev

# Codex
codex plugin marketplace add ./.dev

# Gemini CLI
gemini extensions link ./.dev
```

Cursor: add `.dev` as a local plugin source. Both installs register a server called `bunbird`, so disable the production one while testing. Run `--dev` again after any change to `canonical/`.

## Versions

Claude Code plugin manifests and marketplace entries carry no `version`. A version pins the plugin and commits then ship nothing to existing installs; without one the commit is the version. Codex and Gemini want a version, which comes from `canonical/facts.json`.

The skill names no tools and does not describe the flow. The server owns both: its connect instructions, its tool descriptions, and the author's guide it serves (`read_guide`), which is versioned with the SDK. A copy here would be frozen at install, and in Codex and Gemini until the next `version`. The skill only says when to use Bunbird, how to connect, to follow the server and read its guide first, and a few rules for the agent. Keep it that way: put anything about how the tools work in the server or the guide.

## License

None yet. The team decides before the repository goes public.
