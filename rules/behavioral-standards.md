---
glob: "**"
---

# Behavioral Standards

These rules apply to all work in WordPress projects regardless of task type.
Derived from Karpathy's LLM coding guidelines, adapted for WordPress.

## Think Before Coding

Don't assume. Don't hide confusion. Surface tradeoffs.

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If something is unclear, stop. Name what's confusing. Ask.
- Push back when a simpler approach exists.

## Simplicity First

Minimum code that solves the problem. Nothing speculative.

- No abstractions beyond what the established service/repository/controller
  pattern in this project requires.
- No features beyond what was asked.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

## Surgical Changes

Touch only what you must. Clean up only your own mess.

- Don't "improve" adjacent code, hooks, filters, or formatting.
- Don't refactor code that isn't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.
- Remove only imports/variables/functions that YOUR changes made unused.

The test: every changed line must trace directly to the request.

## Goal-Driven Execution

Define success criteria. Loop until verified.

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Add a REST endpoint" → "Endpoint returns expected response, passes phpcs, nonce and capability verified"

For multi-step tasks, state a brief plan with a verification step per stage.
