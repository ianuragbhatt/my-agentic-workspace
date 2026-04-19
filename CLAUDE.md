# Coding Philosophy

These rules apply to every project. Bias toward caution over speed. For trivial tasks, use judgment.

## Think Before Coding

- State assumptions explicitly before starting. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop and ask rather than guess.

## Simplicity First

- Write the minimum code that solves the problem. Nothing speculative.
- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

## Surgical Changes

- Touch only what the task requires.
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style exactly, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.
- Remove imports/variables/functions that YOUR changes made unused. Leave pre-existing dead code alone.

## Goal-Driven Execution

For multi-step tasks, state a brief plan with verifiable steps before starting:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
```

Transform tasks into verifiable goals:
- "Fix the bug" → write a test that reproduces it, then make it pass
- "Refactor X" → ensure tests pass before and after
