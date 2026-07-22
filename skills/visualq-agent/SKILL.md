---
name: visualq-agent
description: >-
  VisualQ Quality MCP workflows for coding agents — VRT/FRT gates, rolling
  health, onboarding, Jira QA. Use when the user asks about VisualQ, VRT,
  FRT, PR quality gates, merge blockers, or site quality scores.
---

# VisualQ Agent Workflows — Quality MCP for Coding Agents

VisualQ is a **Quality OS**; **`@visualq/mcp`** is the agent interface for Cursor, Claude Code, and CI agents (~44 tools). Install it alongside Playwright MCP:

- **Playwright MCP** — local browser exploration (hands + eyes)
- **VisualQ MCP** — persistent quality memory, gates, baselines, multi-pillar audits, FRT contracts, rolling health

## Setup

```json
{
  "mcpServers": {
    "visualq": {
      "command": "npx",
      "args": ["-y", "@visualq/mcp"],
      "env": {
        "VISUALQ_API_KEY": "vq_org_live_…",
        "VISUALQ_BASE_URL": "https://visualq.ai",
        "VISUALQ_TOOL_PROFILE": "vrt-qa"
      }
    }
  }
}
```

Org agent keys: **Settings → Agent API Keys** (`mcp_full` scope). Project CI keys (`vq_live_…`) are for GitHub Actions only.

Use MCP **prompts**: `setup-health-review`, `pr-quality-gate`, `frt-journey-from-goal`, `onboard-new-site`, `jira-qa`, `jira-tracking-proof`.

Use MCP **resources** for read-only context: `visualq://site-health`, `visualq://latest-failures`, `visualq://frt-step-library`.

## Rolling health (mandatory)

Always use `get_site_health` / `gate_pr_quality` for project KPIs. **Never** treat a single latest run as the site score. Coverage and staleness matter. **Never** use site-wide scores as proof for a specific JIRA ticket.

## Workflow: New project (`onboard-new-site` prompt)

1. `create_project` with `confirm: true`
2. `crawl_site` (async — poll `get_job_status`)
3. `create_scenario` for key URLs from crawl results
4. `run_baseline` with `confirm: true`
5. `create_frt_scenario` for checkout/login or primary flow (`goal` + `confirm: true`)
6. `check_setup_health` — summarize blockers

## Workflow: PR quality gate (`pr-quality-gate` prompt)

1. `gate_pr_quality` — structured verdict (VRT + FRT + rolling health)
2. If `blockMerge: true`, run targeted fixes:
   - VRT: `get_run_failures` → `explain_vrt_failure`
   - FRT: `frt_explain_failure` on failed scenarios
3. Re-run: `run_vrt` and/or `run_frt_feature` with `confirm: true`
4. `gate_pr_quality` again until green or human override

## Workflow: FRT from user story (`frt-journey-from-goal` prompt)

1. `frt_find_scenarios(query=<goal>)` — reuse when strong match / same ticket
2. Optional: `frt_search_step_library` / `frt_inspect_page`
3. **`create_frt_scenario` with `confirm: true` immediately** — do not paste Gherkin for manual import
4. Optional: `run_frt_feature` with `confirm: true`
5. `frt_explain_failure` on any red steps

## Workflow: Jira ticket QA (`jira-qa` prompt)

1. Parse ticket: acceptance criteria → test scope
2. `frt_find_scenarios` + `frt_get_feature_groups` — check existing coverage
3. Create or update:
   - VRT: `create_scenario` with ticket id in label
   - FRT: `create_frt_scenario` with ticket id in name
4. `run_vrt` + `run_frt_feature` with `confirm: true`
5. `gate_pr_quality` — attach verdict to PR comment via `post_pr_comment` if configured

## Workflow: JIRA tracking proof (`jira-tracking-proof` prompt)

1. Read ticket via JIRA MCP → action-only `reproGoal`
2. `tracking_prove_jira_ticket` (`confirm: true`) — one call; poll `get_job_status`
3. If proven + `mayClaimTicketFixed`: paste `jiraMarkdown` verbatim
4. If not: **NON** + follow `investigationLadder` (no `run_full_audit`)

## Workflow: Pre-merge VRT only

1. `run_vrt` with environment staging
2. `wait_for_run`
3. `get_run_failures` — block if failed > 0
4. `get_site_health` for rolling quality context

## Workflow: Diagnose VRT failure

1. `get_run_failures`
2. `get_diff_stats` on worst scenario
3. `explain_vrt_failure`
4. `frt_inspect_page` if selector/content issue
5. `create_comparison_rule` or `create_content_rule` with `confirm: true`

## Workflow: Full quality audit

Use `run_full_audit` with `pillars[]` (a11y, perf, seo, security, tracking, functional) — then `wait_for_run` / `get_site_health`. There are no separate `run_a11y` / `run_tracking` MCP tools.

## FRT philosophy (no repair at runtime)

- Fix failures at source: coach docs, binding, step catalog — not silent selector swaps
- `frt_heal_step_def` proposes patches; user/agent must `confirm: true` to persist
- System step defs (`system.*`) are never auto-healed at runtime

## Mobile (Maestro complement)

VisualQ covers web + responsive viewports. For native mobile, use **Maestro MCP** alongside VisualQ.
