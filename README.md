# Bunbird plugins

Distribution package for [Bunbird](https://bunbird.com): per-client plugins that connect an AI agent to Bunbird's two hosted MCP servers, plus one skill that tells the agent how to use them. There is no server code here.

| Server | URL | Auth | For |
|---|---|---|---|
| `bunbird-build` | https://mcp.bunbird.com/build | none | reading the guide, creating, building and previewing drafts |
| `bunbird` | https://mcp.bunbird.com/mcp | OAuth | the signed-in person: saving, resuming and publishing drafts |

A draft's `draftToken` works on both.

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

**Accept the connector after install.** A plugin carries the MCP servers, but the client asks the person to accept them, and `bunbird` also needs a sign-in. Until that is done the skill loads with no tools behind it. Per-client steps are in each plugin's README under `plugins/`.

## Layout

- `canonical/` is the only hand-edited source: `facts.json` (names, URLs, servers), `skill.md` (the skill body), `platform-notes/<client>.md` (client-specific extras).
- `scripts/build.mjs` generates everything else: `plugins/`, the three marketplace catalogs, `gemini-extension.json`, `GEMINI.md`, `AGENTS.md`, root `SKILL.md`, and `.gitattributes`.

## Editing

1. Edit files under `canonical/` only. Never edit a generated file.
2. `node scripts/build.mjs`
3. `node scripts/build.mjs --check` (fails on any drift, or any missing or extra file in `plugins/`)

`npm run build` and `npm run check` do the same. Node 20 or newer; the generator has no dependencies.

Run `bun install` once after cloning: it installs lefthook, whose pre-commit hook runs the drift check on every commit. The org runs checks locally, so there is no CI workflow.

## Versions

Claude Code plugin manifests and marketplace entries carry no `version`. A version pins the plugin and commits then ship nothing to existing installs; without one the commit is the version. Codex and Gemini want a version, which comes from `canonical/facts.json`.

The skill avoids depending on tool names, because clients cache the tool list at install and a rename breaks live installs. Add tools server-side rather than renaming.

## License

None yet. The team decides before the repository goes public.
