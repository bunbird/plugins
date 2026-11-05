Adding the marketplace does not install the plugin: open Codex, run `/plugins` and install Bunbird. Then approve the sign-in when Codex asks for `bunbird`. If it does not ask, run `codex mcp login bunbird` once.

To connect by hand instead of installing the plugin, add this to `config.toml`:

```toml
[mcp_servers.bunbird]
url = "{{url}}"
```

Then run `codex mcp login bunbird`.
