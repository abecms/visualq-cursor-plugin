# VisualQ — Cursor & Claude Code Plugin

Official agent bundle for [VisualQ](https://visualq.ai): MCP server config, skills, and rules for QA workflows in Cursor and Claude Code.

**Marketplace display name:** VisualQ · **Identifier:** `visualq` · **Version:** 1.1.0

## What's included

| Component | Purpose |
|-----------|---------|
| **MCP** (`mcp.json`) | `@visualq/mcp` via npx — ~44 QA tools |
| **Skills** | `visualq-agent`, `vrt-qa`, `frt-qa`, `tracking-qa` |
| **Rule** | `visualq-qa-agent.mdc` — conventions (`project`, `confirm`, rolling health) |
| **Logo** | `assets/logo.svg` (+ PNG) |
| **MCP prompts** | Bundled in `@visualq/mcp` (`pr-quality-gate`, `onboard-new-site`, `jira-tracking-proof`, …) |

## Prerequisites

1. [VisualQ account](https://visualq.ai)
2. **Org agent API key** — Settings → Agent API Keys → `mcp_full` scope (`vq_org_live_…`)
3. Node.js 18+ (for stdio MCP)

## Install options

### A — Cursor Marketplace (recommended)

1. Open **Cursor → Customize → Browse Marketplace**
2. Search **VisualQ**
3. Enter your org agent key when prompted (`VISUALQ_API_KEY`)
4. Restart Cursor

Submit / re-index: [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)

### B — One-command CLI

```bash
npx @visualq/setup-agent cursor --key vq_org_live_YOUR_KEY --project my-site
```

### C — Manual (local plugin test)

```bash
cp -R . ~/.cursor/plugins/local/visualq
# Configure Plugins → VisualQ → set VISUALQ_API_KEY
# Or for a local mcp.json without variables, paste the key into env
# Developer: Reload Window in Cursor
```

### D — Claude Code

```bash
/plugin install visualq
# Or from this repo:
/plugin install github:abecms/visualq-cursor-plugin
```

Configure `userConfig.apiKey` in plugin settings.

### E — Hosted MCP (no Node)

Use `mcp-hosted.json` — connects to `https://visualq.ai/api/mcp` with `X-API-Key`.

## First conversation

After install and IDE restart:

> Run prompt **pr-quality-gate** for project **my-site**

Or in plain language:

> Can I merge this PR? Check VisualQ quality on staging.

## Tool profiles

Set `VISUALQ_TOOL_PROFILE` in MCP env (or plugin Configure):

| Profile | Use case |
|---------|----------|
| `vrt-qa` | Pre-merge VRT (default) |
| `frt-qa` | Gherkin / functional journeys |
| `tracking-qa` | Analytics coverage + JIRA proof |
| `full` | All tools |

## Security

- Never commit `vq_org_live_` keys to git
- Marketplace installs use `${VISUALQ_API_KEY}` variables (configured in Cursor Plugins → Configure)
- Org keys are for IDE agents; use project CI keys (`vq_live_`) in GitHub Actions only

## Links

- [MCP documentation](https://visualq.ai/docs/integrations/mcp)
- [npm @visualq/mcp](https://www.npmjs.com/package/@visualq/mcp) (current: **1.0.6+**)
- [Marketplace submission guide](./docs/MARKETPLACE.md)

## License

MIT — see [LICENSE](./LICENSE)
