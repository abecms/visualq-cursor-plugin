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
2. `frt_search_step_library` — reuse canonical step patterns.
3. Optional: `frt_inspect_page` / `frt_resolve_page` to ground labels and selectors.
4. `frt_save_feature_draft` with `confirm: true` — pass `project`, `name`, `gherkin`, `baseUrl`, `environment`.
5. `frt_compile_feature` with `project` + `featureId`.
6. `run_frt_feature` with `confirm: true` if they asked to run or test.

**Never** output Gherkin for copy-paste, import tables, or "commande MCP" instructions. Saving in VisualQ is the deliverable.

## Conventions

1. Always pass `project` on every tool.
2. Mutations need `confirm: true` (`frt_save_feature_draft`, `run_frt_feature`, `frt_heal_step_def`).
3. Async tools (`frt_propose_journey`) → poll `get_job_status`.

## No repair at runtime

- Fix coach docs, binding, step catalog — not silent selector swaps
- `system.*` step defs are never auto-healed at runtime
- `frt_heal_step_def` requires explicit `confirm: true` to persist

## Debug failure

1. `frt_explain_failure` with runId / step index
2. `frt_get_step_def` — inspect persisted body
3. `frt_inspect_page` if DOM/selector issue
