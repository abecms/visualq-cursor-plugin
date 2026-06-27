# Marketplace submission checklist

## Cursor official marketplace

1. Ensure repo is **public** on GitHub: `abecms/visualq-cursor-plugin`
2. Verify structure:
   - `.cursor-plugin/plugin.json` at plugin root
   - `skills/`, `rules/`, `mcp.json`
3. Test locally:
   ```bash
   cp -R . ~/.cursor/plugins/local/visualq
   # Reload Cursor → Customize → verify MCP + skills
   ```
4. Submit at [https://cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
5. Include in submission notes:
   - Open source MIT
   - No telemetry; calls visualq.ai API only with user-provided key
   - Screenshots: Customize panel, sample chat (PR gate)

**Review:** Manual — expect 1–3 weeks.

## cursor.directory (community)

1. Go to [https://cursor.directory/plugins/new](https://cursor.directory/plugins/new)
2. Authenticate with GitHub
3. Paste repo URL — auto-detects `.cursor-plugin`

## Claude Code

1. Publish `.claude-plugin/marketplace.json` on the same repo
2. Users: `/plugin install github:abecms/visualq-cursor-plugin`
3. Document in [visualq docs](https://visualq.ai/docs/integrations/mcp)

## Post-launch

- Tag releases semver (`v1.0.0`)
- Sync skills from `@visualq/agent-skills` when workflows change
- Bump `@visualq/mcp` minimum version in README when breaking
