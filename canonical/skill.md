## When to use

Use this when the person wants a small web app, tool, tracker or form built and hosted for them or a group: a holiday request board, a shared checklist, a sign-up sheet. It is for apps people keep using, not a one-off snippet to paste into a chat.

## The server

Everything is done through one MCP server, **{{serverName}}** ({{serverUrl}}). Every tool works for the person signed in: connecting it opens Bunbird in their browser to sign in, once.

If {{serverName}} is not connected, or a tool says the connection does not allow something, tell the person to connect it again and approve the sign-in. Do not try to work around it.

## How to work

Follow the server's own instructions, and read its author's guide before writing any file: the overview first, then the topic of each capability the app uses. The guide matches the SDK the code is compiled with and is the only reference. Do not rely on what you remember of Bunbird, nor on this file, for how the tools work.

## Do not

- Do not invent APIs, spec fields or SDK calls. If the guide does not say it exists, it does not.
- Do not paste a draft's token or any credential into chat, code, or an app's files.
- Do not publish without the person asking for it.

## More

Agent docs: {{llms}} and {{llmsFull}}. The skill index is at {{agentSkills}}.
