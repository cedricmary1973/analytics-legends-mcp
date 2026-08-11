# Client configuration

Two files, one difference: the second sends a subscriber key and therefore opens the
five gated tools. Without a key the other eleven answer normally — the gated five are
still listed, and refuse **by name** so an agent can see what it is missing rather than
believing the surface is smaller than it is.

| file | opens |
|---|---|
| `claude-desktop.json` | the 11 public tools, no account needed |
| `claude-desktop-subscriber.json` | all 16, with a personal `alk_…` key |

A key comes from an [analyticslegends.ai](https://analyticslegends.ai/pricing/) account
(Consultant or above), or from the MCP Pass (€39/month) — machine access only.

**Claude Code needs no file:**

```bash
claude mcp add --transport http analytics-legends https://analyticslegends.ai/mcp
```
