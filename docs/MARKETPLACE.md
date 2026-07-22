# Marketplace submission checklist

## Cursor official marketplace

1. Ensure repo is **public** on GitHub: `abecms/visualq-cursor-plugin`
2. Verify structure:
   - `.cursor-plugin/plugin.json` at plugin root (`name`: `visualq`, `displayName`: `VisualQ`, `logo`: `assets/logo.svg`)
   - `skills/`, `rules/`, `mcp.json`, `assets/logo.svg`
   - `variables` declare `VISUALQ_API_KEY` (required); `mcp.json` uses `${VISUALQ_API_KEY}` only
3. Test locally:
   ```bash
   cp -R . ~/.cursor/plugins/local/visualq
   # Reload Cursor → Customize → verify MCP + skills + logo
   ```
4. Submit at [https://cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
5. Include in submission notes:
   - **Display name:** VisualQ
   - Open source MIT
   - No telemetry; calls visualq.ai API only with user-provided org agent key
   - Logo: `assets/logo.svg`
   - Screenshots: Customize panel, sample chat (PR gate / JIRA tracking proof)

**Review:** Manual — expect 1–3 weeks. After updates, request a re-index from Anysphere (same publish URL / publisher flow).

## cursor.directory (community)

1. Go to [https://cursor.directory/plugins/new](https://cursor.directory/plugins/new)
2. Authenticate with GitHub
3. Paste repo URL — auto-detects `.cursor-plugin`

## Claude Code

1. Publish `.claude-plugin/marketplace.json` on the same repo
2. Users: `/plugin install github:abecms/visualq-cursor-plugin`
3. Document in [visualq docs](https://visualq.ai/docs/integrations/mcp)

## Post-launch

- Tag releases semver (`v1.1.0`)
- Keep skills aligned with `@visualq/mcp` server instructions (especially `create_frt_scenario` + JIRA investigation ladder)
- Bump plugin `version` when skills/rules/MCP env change
