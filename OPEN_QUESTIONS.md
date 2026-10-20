# Open Questions

Unresolved items across the Cascade-PIE system. Each item is tracked until closed by test, analysis, or design change.

---

### 1. Format

| Field | Meaning |
|---|---|
| ID | Sequential identifier |
| Item | The question |
| Domain | Pipeline, memory, UI, cost, safety |
| Impact | What is blocked |
| Method | Test, analysis, design |
| Status | Open, closed |

---

### 2. Register

| ID | Item | Domain | Impact | Method | Status |
|---|---|---|---|---|---|
| OQ-01 | Actual failure rate per stage under load | Pipeline | Reliability | Load test | Open |
| OQ-02 | Appropriate timeout per provider | Pipeline | UX | Measurement | Open |
| OQ-03 | Behavior when primary and backup both fail | Pipeline | Reliability | Test | Open |
| OQ-04 | Cost of degraded output vs retry | Cost | Economics | Analysis | Open |
| OQ-05 | Log rotation policy | Pipeline | Maintenance | Design | Open |
| OQ-06 | Is 0.1 exploration rate optimal | Memory | Learning | Simulation | Open |
| OQ-07 | Feedback weighting by user trust | Memory | Fairness | Design | Open |
| OQ-08 | Contradictory feedback handling | Memory | Correctness | Design | Open |
| OQ-09 | Fixed question type list sufficiency | Memory | Coverage | Analysis | Open |
| OQ-10 | Memory corruption recovery | Memory | Reliability | Test | Open |
| OQ-11 | Gatekeeper accuracy on ambiguous queries | Pipeline | Quality | Benchmark | Open |
| OQ-12 | Judge consistency across rounds | Pipeline | Stability | Benchmark | Open |
| OQ-13 | Advocate/Critic same-model vs different-model | Pipeline | Quality vs cost | A/B test | Open |
| OQ-14 | Latency in production (vs estimate) | Cost | UX | Measurement | Open |
| OQ-15 | Token cost in production (vs estimate) | Cost | Economics | Measurement | Open |

---

### 3. Critical Path

Items that block implementation of the first working prototype:

| Item | Blocks |
|---|---|
| OQ-11 | Gatekeeper threshold |
| OQ-12 | Judge loop stability |
| OQ-14 | UX viability |
| OQ-15 | Cost viability |

Items not on critical path can be resolved after the prototype runs.

---

### 4. Governance

Register is reviewed after each prototype iteration. Closed items remain with a resolution reference. No item is deleted.