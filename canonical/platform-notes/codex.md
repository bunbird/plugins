Install the plugin, then accept the connectors Codex shows after install and sign in to `bunbird` when asked. `bunbird-build` needs no sign-in.

To connect by hand instead of installing the plugin, add this to `config.toml`:

```toml
[mcp_servers.bunbird-build]
url = "{{buildUrl}}"

[mcp_servers.bunbird]
url = "{{accountUrl}}"
```
