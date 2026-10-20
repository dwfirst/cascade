# Error Handling - Async Execution, Timeouts, Fallback

This document defines how the pipeline behaves when a model call fails, times out, or returns garbage.

---

### 1. Async Execution

| Parameter | Value |
|---|---|
| Framework | aiogram 3.x |
| HTTP client | aiohttp |
| Timeout per stage | 30 seconds |

---

### 2. Timeout Policy

| Stage | Timeout | Action on timeout |
|---|---|---|
| Gatekeeper | 10 s | Default to `debate` |
| Decomposer | 15 s | Use raw query |
| Advocate | 30 s | Skip to Critic |
| Critic | 30 s | Skip to Judge |
| Judge | 15 s | Default to `enough` |
| Final Assembly | 30 s | Return debate log |

---

### 3. Fallback Router

If a primary model fails, route to a backup model. The backup list is configuration, not code.

---

### 4. Infinite Loop Prevention

Max rounds: 3. After 3 rounds, Judge is ignored.

---

### 5. UI States

One message per request, edited in place. Buttons: `[Useful]` `[Not useful]` `[Show trace]`.

---

### 6. Open Questions

1. Actual failure rate per stage under load.
2. Appropriate timeout per provider.
3. Behavior when both primary and backup fail.
4. Cost of degraded output vs retry.
5. Log rotation policy.
