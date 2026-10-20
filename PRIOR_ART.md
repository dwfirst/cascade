# Prior Art - Related Work and Differentiation

This document lists prior work in multi-agent debate and explains how Cascade-PIE differs.

---

### 1. Related Systems

| System | Year | Scope | Reference |
|---|---|---|---|
| SELENE | 2026 | Adaptive debate gating | EACL 2026 |
| SEIMAD | 2026 | Socratic debate with roles | Expert Systems with Applications |
| Persuasio | 2026 | Formal persuasion protocol | GitHub |
| DebateQD | 2025 | Strategy evolution via quality-diversity | NeurIPS 2025 |
| Multi-Agent Debate survey | 2026 | Taxonomy of 141 studies | arXiv |

---

### 2. Comparison

| Dimension | SELENE | SEIMAD | Persuasio | DebateQD | Cascade-PIE |
|---|---|---|---|---|---|
| When debate | Adaptive | Always | Always | Always | Adaptive |
| Structure | Free | Socratic | Formal | Free | Socratic + formal |
| Learning from outcomes | No | No | No | Yes (evolution) | Yes (memory) |
| Stages | 1 | 3 | 1 | 1 | 6 |
| Model-agnostic | Yes | Yes | Yes | Partial | Yes |
| Human feedback loop | No | No | No | No | Yes |

---

### 3. Differentiation

Cascade-PIE differs from prior work in three specific ways.

1. Staged pipeline. Prior debate systems are single-stage: they run a debate and return. Cascade-PIE runs a pipeline: gatekeeper, decomposer, debate chamber, judge, assembly. Each stage has a distinct role.

2. Outcome-based memory. SELENE, SEIMAD, and Persuasio do not learn from outcomes. DebateQD evolves strategies, but through quality-diversity search, not user feedback. Cascade-PIE updates strategy weights from explicit user feedback (useful / not useful).

3. Human-in-the-loop feedback. The user is not a passive recipient. They mark answers as useful or not, and that mark changes future behavior. No prior system combines debate with explicit outcome feedback in a Telegram interface.

---

### 4. What Cascade-PIE Does Not Claim

- It does not claim to be the first multi-agent debate system.
- It does not claim to be the first adaptive debate system.
- It does not claim to outperform flagship models on benchmarks.
- It does not claim to replace human judgment.

It claims to be a specific combination: staged pipeline + Socratic debate + outcome-based memory + Telegram interface.

---

### 5. Open Questions

1. Systematic literature review of Telegram-based AI systems is not performed.
2. Patent landscape for debate orchestration is not surveyed.
3. Direct comparison with SELENE on the same benchmark is not done.