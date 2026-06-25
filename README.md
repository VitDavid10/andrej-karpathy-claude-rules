# Andrej Karpathy's Claude Code Rules: 10 Guidelines

> Behavioral guidelines to eliminate the most common LLM coding mistakes.

A drop-in `CLAUDE.md` / Cursor ruleset extending [Andrej Karpathy's 4 core principles](https://github.com/multica-ai/andrej-karpathy-skills) to **10 practical guidelines** for Claude Code and Cursor. Makes the model think before coding, change only what it must, and prove its work. Built on [Anthropic's official Claude Code best practices](https://docs.anthropic.com/en/docs/claude-code) and refined through production use.

> *"Models make wrong assumptions and keep going without checking. They don't handle confusion or ask for clarification."* — Andrej Karpathy

---

## The 4 core principles

Karpathy's four principles target the biggest failure modes of LLM coding. Everything else in this ruleset builds on them.

| # | Principle | What it prevents |
|---|---|---|
| 1 | **Think Before Coding** | Silent assumptions, hidden confusion, picking one interpretation without asking |
| 2 | **Simplicity First** | Over-engineering: speculative features, single-use abstractions, error handling for impossible cases |
| 3 | **Surgical Changes** | Drive-by refactors, touching code unrelated to the request, deleting code it doesn't understand |
| 4 | **Goal-Driven Execution** | "Make it work" with no success criteria, so the model can't verify or loop on its own |

---

## Beyond the 4: honesty, process, and resource use

The four principles cover *how* the model writes code. Three more classes of problem come from Anthropic's own guidance and from day-to-day production use — and each one earns an extra rule.

**Honesty and confidence.** Anthropic's [guidance on reducing hallucinations](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) is blunt about the fix: let the model say *"I don't know"* instead of guessing, have it answer only when it's genuinely confident, and make it flag when something in the request is unclear rather than quietly assuming. A model that agrees with everything and states guesses as fact is worse than one that pushes back. → rule **5. Honesty** (reinforced by **1. Think Before Coding**).

**Speed vs. clarity.** Charging ahead *feels* efficient, but when the model acts on a task it only half-understood, it burns tokens building the wrong thing — then burns more undoing it. For anything complex, clarifying and planning first is cheaper than redoing. → rule **6. Planning**.

**The model is a resource too.** Using a heavyweight model for a one-line change wastes money; under-powering a deep multi-file refactor wastes time. The right choice isn't fixed — it should be picked, and switched, per task. Making that explicit keeps both cost and capability in check. → rule **10. Model Optimization**.

The remaining rules close predictable gaps in verification, error handling, and quality:

| # | Principle | What it prevents |
|---|---|---|
| 5 | **Honesty** | Confident guesses stated as fact; agreeing with everything; hiding uncertainty |
| 6 | **Planning** | Diving into a multi-file change with no plan, then backtracking |
| 7 | **Verification** | Marking a task "done" without running anything |
| 8 | **Errors** | Stopping at the first error and asking the user to fix it |
| 9 | **Elegance** | Shipping the first rushed solution instead of a clean one |
| 10 | **Model Optimization** | Burning Opus tokens on trivial tasks, or under-powering hard ones |

---

## The rules in full

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing anything:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

### 5. Honesty

**If you don't know, say so.**

- Never hallucinate — if uncertain, state it explicitly at the start of your response.
- Be self-critical. Don't just agree with the user; push back constructively when warranted.
- If you made a mistake, analyze why and avoid repeating it.
- If it was a communication breakdown, point it out so we can resolve it.

### 6. Planning

**For non-trivial tasks, plan before coding.**

A task is non-trivial if it touches multiple files, involves architecture decisions, or has ambiguity. Before writing any code:
1. State the root cause or goal
2. List the changes with file names
3. Flag any open questions

Wait for the user to validate the plan before implementing. Simple or obvious changes: implement directly.

### 7. Verification

**Never mark done without proving it works.**

- Run tests, check logs, verify behavior in edge cases.
- Filter: "Would a senior engineer approve this before shipping?"

### 8. Errors

**Fix errors autonomously. No hand-holding.**

When something fails:
- Use logs, error messages, and failing tests to diagnose.
- Find the root cause — don't patch around it.
- Fix CI tests that fail without asking what they mean.

### 9. Elegance

**Pause before delivering non-trivial work.**

Ask: "Is there a more elegant solution?" If the implementation feels rushed or forced, rewrite it. A clean solution now saves rewrites later.

Only applies to non-trivial changes — don't over-engineer simple fixes.

### 10. Model Optimization

**Use the right model for the task.**

- On **Opus** with a simple task (chat, question, minor change, code search): warn once → *"This doesn't need Opus, use `/model sonnet` to save tokens."*
- On **Sonnet** failing 2+ times on the same problem, or facing deep architectural reasoning across many files: warn → *"Consider `/model opus`."*
- Don't suggest upgrading just because the task is long — only when it requires complex multi-file reasoning.

---

## Install

### Claude Code — Global (all projects)
```
cp CLAUDE.md ~/.claude/CLAUDE.md
```

### Claude Code — Per project
```
cp CLAUDE.md /your-project/CLAUDE.md
```

### Claude Code — as a skill / plugin
The repo ships a skill at `skills/claude-code-rules/SKILL.md` and the `.claude-plugin/` manifests, so it can be installed as a plugin from a marketplace or invoked as a skill.

### Cursor
Copy `.cursor/rules/claude-code-rules.mdc` into your project (it has `alwaysApply: true`), or paste `CURSOR.md` into your existing rules file.

---

## Examples

See **[EXAMPLES.md](EXAMPLES.md)** for a wrong-vs-right example of every rule, using a real-time multiplayer game server (Node + WebSocket).

---

## Tradeoff

These guidelines bias toward caution over speed. The goal is reducing costly mistakes on non-trivial work, not slowing down simple tasks.

**You'll know the rules are working when:**
- Diffs have fewer unnecessary changes — only what was asked
- Clarifying questions come *before* implementation, not after mistakes
- The model stops and names confusion instead of silently assuming
- First drafts don't need rewrites due to overcomplication
- Edge cases and bugs are caught before marking done, not after
- The right model tier is used for the right task — no wasted tokens

---

## Credits

- [Andrej Karpathy](https://github.com/multica-ai/andrej-karpathy-skills) — the original 4 principles this builds on
- [Anthropic Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code) — official best practices and memory system
- [Anthropic: reducing hallucinations](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — honesty and confidence guidance

---

## License

MIT
