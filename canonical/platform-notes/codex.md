Adding the marketplace alone does not install the plugin: `codex plugin add bunbird@bunbird` does, or open Codex, run `/plugins` and install Bunbird. Then approve the sign-in when Codex asks for `bunbird`. If it does not ask, run `codex mcp login bunbird` once.

The Codex IDE extension does not load plugins. To connect it, or to connect by hand instead of installing the plugin, add this to `config.toml`:

```toml
[mcp_servers.bunbird]
url = "{{url}}"
```

Then run `codex mcp login bunbird`.
