# Pipeline - Debate Stages and Roles

This document defines the stages of the Cascade-PIE pipeline. Each stage has a role, a model, and a prompt.

---

### 1. Gatekeeper

| Parameter | Value |
|---|---|
| Role | Decide if debate is needed |
| Model | Fast, cheap (e.g., 8B-class) |
| Input | User query |
| Output | `debate` or `direct` |

Prompt:
You are a gatekeeper. Decide if the following question needs a structured debate or can be answered directly.

A question needs debate if:

It has multiple valid interpretations.

It involves a value judgment.

It has no single factual answer.

The user asks "why" or "compare".

A question does not need debate if:

It is a factual lookup.

It is a simple calculation.

It is a greeting or small talk.

Output only: "debate" or "direct".

text

---

### 2. Decomposer

| Parameter | Value |
|---|---|
| Role | Split the question into claims and variables |
| Model | Fast, cheap |
| Input | User query |
| Output | Structured list of claims, variables, intent |

Prompt:
You are a semantic decomposer. Break the following question into:

Core claims (what is being asserted or asked)

Hidden variables (what is assumed but not stated)

Intent (what the user actually wants)

Output a structured markdown list. Do not answer the question.

text

---

### 3. Advocate

| Parameter | Value |
|---|---|
| Role | Argue for the strongest interpretation |
| Model | Strong reasoning model |
| Input | Decomposed claims |
| Output | Arguments for, with evidence |

Prompt:
You are an advocate. Argue for the strongest interpretation of the following claims. Provide concrete evidence, examples, and reasoning. Do not hedge. Do not say "on the other hand". Take a position and defend it.

text

---

### 4. Critic

| Parameter | Value |
|---|---|
| Role | Argue against, find weaknesses |
| Model | Strong reasoning model (may be same as Advocate, different prompt) |
| Input | Decomposed claims + Advocate output |
| Output | Counter-arguments, gaps, contradictions |

Prompt:
You are a critic. Your job is to find weaknesses in the following argument. Identify:

Logical gaps

Missing evidence

Alternative interpretations

Contradictions

Do not be polite. Be specific. If the argument is strong, say so, but find at least one weakness.

text

---

### 5. Judge

| Parameter | Value |
|---|---|
| Role | Evaluate if debate is sufficient |
| Model | Strong reasoning model |
| Input | Advocate + Critic outputs |
| Output | `enough` or `more`, plus reasoning |

Prompt:
You are a judge. Evaluate the following debate. Decide:

Is the debate sufficient to answer the question?

If not, what specific point needs another round?

Output: "enough" or "more", followed by one sentence of reasoning.

text

---

### 6. Final Assembly

| Parameter | Value |
|---|---|
| Role | Synthesize debate into final answer |
| Model | Strong flagship model |
| Input | Decomposed claims + Advocate + Critic + Judge |
| Output | Final answer + confidence + trace |

Prompt:
You are a synthesizer. Produce a final answer based on the following debate. Include:

The answer itself

Confidence level (low / medium / high)

A trace of which arguments were accepted and which were rejected

Do not include meta-commentary. Do not say "based on the debate". Just answer.

text

---

### 7. Outcome Tracker

| Parameter | Value |
|---|---|
| Role | Collect user feedback |
| Interface | Telegram inline buttons |
| Output | `useful` / `not_useful` / `show_trace` |

After the final answer, the bot shows three buttons:

- `[Useful]` — increments success counter for the strategy used
- `[Not useful]` — increments failure counter
- `[Show trace]` — displays the full debate log

---

### 8. Strategy Memory

| Parameter | Value |
|---|---|
| Role | Learn which strategies work for which question types |
| Storage | Local JSON or SQLite |
| Input | Question type + strategy used + outcome |
| Output | Updated weights |

See `STRATEGY_MEMORY.md` for details.

---

### 9. Model Selection

| Stage | Model class | Rationale |
|---|---|---|
| Gatekeeper | Small, fast | Binary decision, no reasoning needed |
| Decomposer | Small, fast | Structured extraction |
| Advocate | Strong reasoning | Quality of argument matters |
| Critic | Strong reasoning | Quality of critique matters |
| Judge | Strong reasoning | Evaluation needs judgment |
| Final Assembly | Flagship | Final answer quality matters |

Advocate and Critic may use the same model with different prompts. This is cheaper and simpler. The architecture does not require different models.

---

### 10. Cost and Latency

| Stage | Approximate tokens | Latency |
|---|---|---|
| Gatekeeper | 100 | < 1 s |
| Decomposer | 300 | < 2 s |
| Advocate | 800 | < 5 s |
| Critic | 800 | < 5 s |
| Judge | 300 | < 2 s |
| Final Assembly | 1000 | < 8 s |
| **Total** | **~3300** | **~20 s** |

This is an estimate, not a measurement. Actual cost depends on the model provider.

For simple questions (Gatekeeper returns `direct`), total is ~500 tokens and < 3 s.
