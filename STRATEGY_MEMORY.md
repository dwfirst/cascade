# Strategy Memory - Learning from Outcomes

This document defines how Cascade-PIE learns from user feedback. The system does not train a model. It updates a table of strategy weights.

---

### 1. What Is Stored

Each entry is a tuple:

(question_type, strategy, success_count, failure_count)

| Field | Type | Description |
|---|---|---|
| question_type | string | Category assigned by Decomposer |
| strategy | string | Pipeline configuration used |
| success_count | integer | Times user marked "useful" |
| failure_count | integer | Times user marked "not useful" |

No raw text is stored. No user identity is stored. Only aggregate counts.

---

### 2. Question Types

The Decomposer assigns a type from a fixed list.

| Type | Description |
|---|---|
| factual | Lookup, definition, date |
| analytical | Compare, evaluate, explain why |
| normative | Should, ought, value judgment |
| predictive | Will, forecast, trend |
| procedural | How to, steps, method |

The type is a single label. If ambiguous, Decomposer picks the closest.

---

### 3. Strategies

A strategy is a pipeline configuration.

| Strategy | Description |
|---|---|
| direct | No debate, direct answer |
| single_round | One round of Advocate/Critic, no Judge loop |
| multi_round | Up to 3 rounds with Judge |
| advocate_heavy | Longer Advocate output, shorter Critic |
| critic_heavy | Shorter Advocate, longer Critic |

The strategy is chosen by the Gatekeeper based on question type and current weights.

---

### 4. Weight Update

After each request, the strategy used is updated.

weight = success_count / (success_count + failure_count + 1)

The +1 prevents division by zero and biases new strategies toward exploration.

| Outcome | Update |
|---|---|
| Useful | success_count += 1 |
| Not useful | failure_count += 1 |
| No feedback | No update |

No feedback is neutral. It does not count as failure.

---

### 5. Strategy Selection

When a new question arrives, the Gatekeeper picks the strategy with the highest weight for that question type.

If no entry exists, the default is single_round.

Exploration: with probability 0.1, a random strategy is chosen instead of the highest-weight one. This prevents locking into a local optimum.

---

### 6. Storage

Stored as a single JSON file.

```json
{
  "factual": {
    "direct": {"success": 12, "failure": 1},
    "single_round": {"success": 3, "failure": 0}
  },
  "analytical": {
    "single_round": {"success": 8, "failure": 2},
    "multi_round": {"success": 15, "failure": 3}
  }
}
Read at startup, written after each feedback event.

7. Inspection
Operation	Method
Inspect weights	/memory command
Reset all	/memory reset (requires confirmation)
Export	/memory export (returns JSON file)
8. Limits
Limit	Value
Max question types	5 (fixed)
Max strategies per type	5 (fixed)
Max entries	25
Max file size	10 KB
The memory is a lookup table, not a model.

9. Open Questions
Is 0.1 exploration rate optimal, or should it decay?

Should feedback be weighted by user trust level?

How does the system handle contradictory feedback?

Is a fixed question type list sufficient?

What happens when memory is lost or corrupted?
---