# Andrej Karpathy Rules — Claude Code & Cursor Guidelines

> Behavioral guidelines to eliminate the most common LLM coding mistakes.

A drop-in `CLAUDE.md` / Cursor ruleset that makes the model **think before coding, change only what it must, and prove its work**. Built on [Andrej Karpathy's LLM coding guidelines](https://github.com/multica-ai/andrej-karpathy-skills), extended with [Anthropic's official Claude Code best practices](https://docs.anthropic.com/en/docs/claude-code) and refined through production use building real-time multiplayer games (WebSocket servers, Solana blockchain).

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

## Beyond the 4: problems we kept hitting

The four principles cover *how* the model writes code. But in real day-to-day use, other recurring problems showed up — about honesty, process, and resource use. Each one motivated an extra rule:

| Problem observed in practice | Rule added |
|---|---|
| Model agrees with everything and states guesses as facts | **5. Honesty** |
| Dives into a multi-file change with no plan, then backtracks | **6. Planning** |
| Marks a task "done" without running anything | **7. Verification** |
| Stops at the first error and asks the user to fix it | **8. Errors** |
| Main context fills up with exploration and search noise | **9. Subagents** |
| Ships the first rushed solution instead of a clean one | **10. Elegance** |
| Burns Opus tokens on trivial tasks, or under-powers hard ones | **11. Model Optimization** |
| Loses critical state (files, decisions, TODOs) when context compresses | **12. Compaction** |

---

## The rules in full

### 1. Think Before Coding
*Don't assume. Don't hide confusion. Surface tradeoffs.* State assumptions explicitly; if uncertain, ask. Present multiple interpretations instead of silently picking one. If a simpler approach exists, say so. If something is unclear, stop and name what's confusing.

### 2. Simplicity First
*Minimum code that solves the problem. Nothing speculative.* No features beyond what was asked, no abstractions for single-use code, no unrequested "flexibility", no error handling for impossible scenarios. If 200 lines could be 50, rewrite it. The test: *"Would a senior engineer say this is overcomplicated?"*

### 3. Surgical Changes
*Touch only what you must. Clean up only your own mess.* Don't improve adjacent code, comments, or formatting. Don't refactor what isn't broken. Match existing style. Remove orphans **your** changes created — but leave pre-existing dead code alone (flag it, don't delete it). Every changed line should trace directly to the request.

### 4. Goal-Driven Execution
*Define success criteria. Loop until verified.* Turn vague tasks into verifiable goals: "fix the bug" → "write a test that reproduces it, then make it pass". State a brief plan for multi-step work. Strong criteria let the model loop independently; weak criteria ("make it work") force constant clarification.

### 5. Honesty
*If you don't know, say so.* Never hallucinate — state uncertainty up front. Be self-critical; push back constructively instead of just agreeing. When a mistake happens, analyze why and avoid repeating it; if it was a communication breakdown, surface it.

### 6. Planning
*For non-trivial tasks, plan before coding.* A task is non-trivial if it touches multiple files, involves architecture decisions, or has ambiguity. Present (1) root cause/goal, (2) changes with file names, (3) open questions — and wait for validation. Simple changes: implement directly.

### 7. Verification
*Never mark done without proving it works.* Run tests, check logs, verify edge cases. Filter: *"Would a senior engineer approve this before shipping?"*

### 8. Errors
*Fix errors autonomously. No hand-holding.* Use logs, error messages, and failing tests to diagnose. Find the root cause — don't patch around it. Fix failing CI tests without asking what they mean.

### 9. Subagents
*Delegate research and exploration.* Don't saturate the main context with investigation. Send codebase exploration, research, and multi-step lookups to subagents; push maximum load to them on hard problems.

### 10. Elegance
*Pause before delivering non-trivial work.* Ask: *"Is there a more elegant solution?"* If the implementation feels rushed or forced, rewrite it. Applies only to non-trivial changes — don't over-engineer simple fixes.

### 11. Model Optimization
*Use the right model for the task.* On Opus with a simple task, warn once to switch to `/model sonnet`. On Sonnet failing repeatedly or facing deep multi-file reasoning, suggest `/model opus`. Don't upgrade just because a task is long — only when it needs complex reasoning.

### 12. Compaction
*Preserve what matters when the context window compresses.* When the conversation is compacted, the summary should retain: current task and status, modified files, errors and how they were resolved, pending TODOs, architecture decisions, and the active git branch/feature.

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

### Cursor
Copy `CURSOR.md` to `.cursor/rules/guidelines.mdc` inside your project, or paste the contents into your existing rules file.

---

## These rules are working if…

Fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come **before** implementation rather than after mistakes.

---

## Credits

- [Andrej Karpathy](https://github.com/multica-ai/andrej-karpathy-skills) — the original 4 principles this builds on
- [Anthropic Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code) — official best practices and memory system

---

## License

MIT
