# Security policy

## Reporting a vulnerability

Do not report a suspected vulnerability in a public issue, pull request or chat channel.

Use this repository's **Security** tab to open a private security advisory, or write to support@bunbird.com. Include what you found, the affected file or commit, steps to reproduce and the impact you believe it has.

We acknowledge the report, assess it and agree a disclosure date with you. Do not publish details until a fix is out or that date has passed.

## Scope

This repository holds plugin packaging only: manifests, a skill and the address of Bunbird's hosted MCP connector. There is no server code here. Report weaknesses in the connector or the platform the same way; they are handled together.

Fixes are made on `main`. Clients pick them up on their next plugin refresh.
