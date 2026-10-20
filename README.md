# Cascade-PIE

A Telegram bot that runs a structured debate before answering. It decomposes the question, argues both sides, judges the result, and learns from user feedback.

---

## Status

This repository is an architecture specification, not a product. No implementation exists. No benchmark has been run. The purpose is to define the system before writing code.

| Layer | Maturity |
|---|---|
| Vision | Conceptual |
| Architecture | Specified |
| Implementation | None |

---

## What It Does

1. User sends a question.
2. Gatekeeper decides: simple question or needs debate.
3. If simple: direct answer.
4. If debate: Decomposer splits the question, Advocate argues for, Critic argues against, Judge evaluates.
5. If Judge says "not enough": another round.
6. If Judge says "enough": final answer with confidence and trace.
7. User marks the answer as useful or not useful.
8. Strategy Memory updates: which approach worked for which type of question.

---

## Why Telegram

The bot is a Telegram bot, not a web app. Three reasons:

- **Zero UI cost.** Text and inline buttons are enough for a debate.
- **Mobile by default.** The operator can approve or reject from a phone.
- **No auth layer.** Telegram ID is the identity.

A web dashboard would add frontend, hosting, SSL, and auth without adding value. A Mini App would add days of work for features the debate does not need.

---

## What It Is Not

- Not a general-purpose chatbot.
- Not a replacement for human judgment.
- Not a model. It is an orchestration layer.
- Not vendor-locked. Any model with a chat API can be connected.

---

## Architecture
Input
│
▼
[Gatekeeper] ── simple? ──► [Direct Response] ──► Output
│
└── debate needed
│
▼
[Decomposer] ──► claims, variables, intent
│
▼
[Debate Chamber]
Advocate ──► arguments for
Critic ──► arguments against
│
▼
[Judge] ── enough? ──► no ──► back to Debate Chamber
│
└── yes
│
▼
[Final Assembly] ──► answer + confidence + trace
│
▼
[Outcome Tracker] ──► user feedback
│
▼
[Strategy Memory] ──► update weights

text

---

## Repository Directory

| File | Scope |
|---|---|
| `PIPELINE.md` | Debate stages, roles, prompts |
| `ERROR_HANDLING.md` | Async execution, timeouts, fallback |
| `STRATEGY_MEMORY.md` | How the system learns from feedback |
| `OPEN_QUESTIONS.md` | What is not yet defined |
| `PRIOR_ART.md` | Related work and differentiation |

---

## Prior Art

The system builds on multi-agent debate research: SELENE (adaptive debate gating), SEIMAD (Socratic debate with roles), Persuasio (formal persuasion protocol), DebateQD (strategy evolution). See `PRIOR_ART.md` for details.

The difference: Cascade-PIE combines a staged pipeline, a Socratic debate, and outcome-based learning in one system. No prior system combines all three.

---

## License

Documentation licensed under CC BY-NC-SA 4.0. Code (if and when added) under MIT or Apache 2.0. Core concept and methodology remain under the author's copyright.
