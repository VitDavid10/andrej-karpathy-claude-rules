# Andrej Karpathy Rules — Claude Code & Cursor Guidelines

> Behavioral guidelines to eliminate the most common LLM coding mistakes.

Inspired by [Andrej Karpathy's LLM coding guidelines](https://github.com/multica-ai/andrej-karpathy-skills) and [Anthropic's official Claude Code best practices](https://docs.anthropic.com/en/docs/claude-code). Refined through production use building real-time multiplayer games with WebSocket servers and Solana blockchain.

> *"Models make wrong assumptions and keep going without checking. They don't handle confusion or ask for clarification."* — Andrej Karpathy

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

## The Rules

### 4 core principles (from Karpathy)

| Principle | Summary |
|---|---|
| **Think Before Coding** | Surface assumptions and ambiguity before writing a single line |
| **Simplicity First** | Minimum code. No speculation, no extra abstractions, no impossible-scenario error handling |
| **Surgical Changes** | Touch only what the request requires. Clean only the orphans your changes created |
| **Goal-Driven Execution** | Define verifiable success criteria before implementing |

### Extended rules (Anthropic best practices + production experience)

- **Planning** — non-trivial tasks get a plan first: goal, files, open questions. Wait for validation.
- **Verification** — never mark done without proof. Senior engineer filter.
- **Errors** — fix autonomously, use logs and tests to diagnose. No hand-holding.
- **Subagents** — delegate research, exploration, and analysis to subagents.
- **Elegance** — for non-trivial changes, pause: is there a more elegant solution?
- **Model Optimization** — switch hints between Opus and Sonnet based on task complexity.
- **Compaction** — preserve what matters when the conversation window compresses.

---

## Why these rules exist

LLMs fail in predictable ways:
1. They assume instead of asking
2. They over-engineer (abstractions, error handling, features nobody requested)
3. They touch code they shouldn't (adjacent refactors, style fixes, unrelated dead code)
4. They mark things "done" without verifying

These rules address each failure directly.

---

## Credits

- [Andrej Karpathy](https://github.com/multica-ai/andrej-karpathy-skills) — original 4 principles this builds on
- [Anthropic Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code) — official best practices and memory system
- [Anthropic Claude Code CLAUDE.md guide](https://docs.anthropic.com/en/docs/claude-code/memory) — instructions and memory system

---

## License

MIT
