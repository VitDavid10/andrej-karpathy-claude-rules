# Cursor Rules
# Inspired by Andrej Karpathy's LLM coding guidelines + Anthropic best practices

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

---

## Principles

- **Simplicity**: minimal changes, only what's needed. No features beyond what was asked, no abstractions for single-use code, no error handling for impossible scenarios. If 200 lines could be 50, rewrite it.
- **No laziness**: find root causes, no temp fixes. Senior engineer standard before delivering.
- **Honesty**: if you don't know, say so. If unsure, state it upfront. No hallucinating. Be self-critical.

## Surgical Changes

When editing existing code: don't improve adjacent code, comments, or formatting. Match existing style. Remove imports/variables/functions that YOUR changes made unused — but don't remove pre-existing dead code (flag it instead). Every changed line should trace directly to the user's request.

## Think Before Coding

Before implementing anything:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## Planning

- Non-trivial task (multiple files, architecture decisions, ambiguity): present plan BEFORE touching code — (1) root cause/goal, (2) changes with files, (3) open questions. Wait for validation.
- Plan must be specific enough for the user to spot flaws without reading code.
- Vague instructions → verifiable criteria. Multiple interpretations → present all of them.
- Simple/obvious changes: implement directly.

## Verification

- Never mark done without proving it works. Test edge cases, run tests, check logs.
- Transform tasks into verifiable goals: "fix the bug" → "write a test that reproduces it, then make it pass".
- For multi-step tasks, state a brief plan:
  ```
  1. [Step] → verify: [check]
  2. [Step] → verify: [check]
  ```
- Filter: "Would a senior engineer approve this?"

## Errors

- Fix errors autonomously. No hand-holding. Use logs and tests to diagnose.

## Elegance

- For non-trivial changes: is there a more elegant solution? If it feels rushed, rewrite it.

---

*Inspired by [Andrej Karpathy's LLM coding guidelines](https://github.com/multica-ai/andrej-karpathy-skills) and [Anthropic's Claude Code best practices](https://docs.anthropic.com/en/docs/claude-code).*
