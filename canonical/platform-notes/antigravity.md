Then sign in: in the Antigravity app, open **Customizations**, go to **Installed**, then **MCP Servers**, and press **Authenticate** beside `bunbird`. Sign in to Bunbird in the browser, copy the authorization code it gives you, paste it into **Enter authorization code** and press **Submit**.

To connect by hand instead, add the connector to `~/.gemini/config/mcp_config.json` and sign in the same way:

```json
{"mcpServers":{"bunbird":{"serverUrl":"{{url}}"}}}
```
