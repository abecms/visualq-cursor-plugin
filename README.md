# VisualQ — Cursor & Claude Code Plugin

Official agent bundle for [VisualQ](https://visualq.ai): MCP server config, skills, and rules for QA workflows in Cursor and Claude Code.

## What's included

| Component | Purpose |
|-----------|---------|
| **MCP** (`mcp.json`) | `@visualq/mcp` via npx — 100+ tools |
| **Skills** | `visualq-agent`, `vrt-qa`, `frt-qa`, `tracking-qa` |
| **Rule** | `visualq-qa-agent.mdc` — conventions (`project`, `confirm`, rolling health) |
| **MCP prompts** | Bundled in `@visualq/mcp` (`pr-quality-gate`, `onboard-new-site`, …) |

## Prerequisites

1. [VisualQ account](https://visualq.ai)
2. **Org agent API key** — Settings → Agent API Keys → `mcp_full` scope (`vq_org_live_…`)
3. Node.js 18+ (for stdio MCP)

## Install options

### A — Cursor Marketplace (recommended)

1. Open **Cursor → Customize → Browse Marketplace**
2. Search **VisualQ** (after listing approval)
3. Enter your org agent key when prompted
4. Restart Cursor

Submit this repo: [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)

### B — One-command CLI

```bash
npx @visualq/setup-agent cursor --key vq_org_live_YOUR_KEY --project my-site
```

### C — Manual (local plugin test)

```bash
cp -R . ~/.cursor/plugins/local/visualq
# Edit ~/.cursor/plugins/local/visualq/mcp.json — paste your key
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

Set `VISUALQ_TOOL_PROFILE` in MCP env:

| Profile | Use case |
|---------|----------|
| `vrt-qa` | Pre-merge VRT (default) |
| `frt-qa` | Gherkin / functional journeys |
| `tracking-qa` | Analytics coverage |
| `full` | All tools |

## Security

- Never commit `vq_org_live_` keys to git
- Plugin templates use placeholders only
- Org keys are for IDE agents; use project CI keys (`vq_live_`) in GitHub Actions only

## Links

- [MCP documentation](https://visualq.ai/docs/integrations/mcp)
- [npm @visualq/mcp](https://www.npmjs.com/package/@visualq/mcp)
- [Marketplace submission guide](./docs/MARKETPLACE.md)

## License

MIT — see [LICENSE](./LICENSE)
