---
name: frt-qa
description: >-
  Functional regression tests (FRT) with VisualQ — Gherkin journeys, step
  library, compile and run features. Use when the user mentions FRT, Gherkin,
  user journeys, functional tests, step definitions, or asks to create a
  scenario with MCP / in VisualQ.
---

# VisualQ FRT QA

Set `VISUALQ_TOOL_PROFILE=frt-qa` on the MCP server for a focused FRT toolset.

## Create scenario — EXECUTE (mandatory)

When the user asks to create/make an FRT scenario or feature (with MCP, in VisualQ, in Cursor):

1. **Call MCP immediately** — first tool call, not chat output.
2. `frt_find_scenarios(query=<goal>)` — reuse `featureId` when `matchScore >= 0.85` or same ticket id (BN-XXX).
3. Optional: `frt_search_step_library` / `frt_inspect_page` to ground labels and selectors.
4. `create_frt_scenario(goal=<actions only>, confirm=true)` — update via `featureId` when a ticket scenario exists; never `forceCreate` duplicates.
5. Authenticated flows: say "logged in" / pass `requiresAuth: true` — MCP sets Requires authentication + LOGIN preamble. Do **not** put sign-in steps in the goal.
6. Optional: `startPath` (e.g. `/profile`), `name`. Gherkin: one `When` per scenario; chain further actions with `And`.
7. `run_frt_feature` with `confirm: true` if they asked to run or test.

**Never** output Gherkin for copy-paste, import tables, or "commande MCP" instructions. `create_frt_scenario` persists the feature in VisualQ.

Do **not** put tracking payload verification in the goal — use `tracking_prove_jira_ticket` for JIRA proof.

## Conventions

1. Always pass `project` on every tool.
2. Mutations need `confirm: true` (`create_frt_scenario`, `run_frt_feature`, `frt_heal_step_def`).
3. Async tools → poll `get_job_status`.

## No repair at runtime

- Fix coach docs, binding, step catalog — not silent selector swaps
- `system.*` step defs are never auto-healed at runtime
- `frt_heal_step_def` requires explicit `confirm: true` to persist

## Debug failure

1. `frt_explain_failure` with runId / step index
2. `frt_get_feature` — inspect persisted Gherkin / coverage
3. `frt_inspect_page` if DOM/selector issue

## Prompts

- `frt-journey-from-goal` — create from plain language
